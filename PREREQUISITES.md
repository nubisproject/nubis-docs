## Prerequisites
Before you can contribute to the Nubis project, you'll need to have a set of tools installed.

### GitHub Account
Nubis is entirely hosted on github, and we derive most of our workflows from GitHub's recommended practices.

If you are new to git or GitHub, you are probably better familiarizing yourself with that first. There are a lot of handy tutorials straight from the source [here](https://www.atlassian.com/git/tutorials/)

First you will need to set up an account on GitHub. It doesn't have to be anything special, any old account will do. To set up a GitHub account, click [here](https://github.com/join).

You will also want to set up your ssh keys with GitHub. You can do that [here](https://github.com/settings/ssh).

### git
All our source-control is in Git, so you'll need a git client of some kind. Most distributions include a git client that you can install with your package manager. You can also get git directly from their [downloads site](https://git-scm.com/downloads).

For apt users:
```bash
aptitude install git
```

### AWS CLI
Next, you need to install the AWS CLI tool. You can install it by following the instructions at the top of [this page](http://aws.amazon.com/cli/). For Mac and Linux users you can simply:
```bash
pip install awscli
```

Homebrew users:

```bash
brew install awscli
```

### AWS Credentials
In order to work with AWS you will need to set up some credentials. This is a somewhat involved process as all access to AWS requires utilizing a multi-factor authentication (MFA) device. When your user is added to an account you will receive an encrypted email containing a key pair.

NOTE: These keys need to remain secret. You need to take the utmost care; DO NOT check them into git, send them via unencrypted email, copy them into a pastebin, etcetera.

#### aws-vault
[aws-vault](https://github.com/99designs/aws-vault) is a tool to securely manage AWS API credentials. You will need to download this tool and place it on your path.

Once installed you will use the aws-vault tool to authenticate for all access and actions within AWS. Fist you will need to set up your MFA device.

Lets start by making sure aws-vault is installed and working correctly:
```bash
aws-vault --version
```
This should return something like ```v3.3.0```.

If you are using linux you need to set your backend to use kwallet. I recommend placing this in one of your startup scripts, say ```~/.bashrc```:
```bash
export AWS_VAULT_BACKEND=kwallet
```

The next thing I like to do is set up some local shell variables to make the following commands a bit simpler. Of course you will need to replace the ```ACCOUNT_NAME```, ```ACCOUNT_NUMBER``` and ```LOGIN``` with the ones you received in the user credentials email.
```bash
ACCOUNT_NAME='nubis-training-2016'; ACCOUNT_NUMBER='517826968395'; LOGIN='jcrowe'
```

Now you can run the aws-vault command to set up the account. This will ask you for the 'Access Key ID' and the 'Secret Access Key'. You will need to get those from the user credentials email as well:
```bash
aws-vault --backend=kwallet add ${ACCOUNT_NAME}
```

It should look something like this (keys redacted):
```bash
Enter Access Key ID: AKXXXXXXXXXXXXXXXXXX
Enter Secret Access Key: jF6EfXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Added credentials to profile "nubis-training-2016" in vault
```

Now it is time to create your virtual MFA device:
```bash
aws-vault --backend=kwallet exec -n ${ACCOUNT_NAME} -- aws iam create-virtual-mfa-device --virtual-mfa-device-name ${LOGIN} --outfile ${LOGIN}.png --bootstrap-method QRCodePNG
```

You should see output similar to the following. The number here should correspond to the account number and the 'jcrowe' part should be your user-name (not mine ;-D):
```bash
{
    "VirtualMFADevice": {
        "SerialNumber": "arn:aws:iam::517826968395:mfa/jcrowe"
    }
}
```

You will need to view the ${LOGIN}.png file and use it to configure your MFA application. If you have imagemagic installed you can try ```display $LOGIN.png``` or just open it with any image viewer. I use the [duo mobile app](https://duo.com/solutions/features/two-factor-authentication-methods/duo-mobile), however the [google authenticator app](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=en) works as well. Really, most MFA apps work here so if you have one you already use or prefer it will probably be just fine.

To finish up, you need to enable the mfa device. This step is basically proving that you are you and everything is configured correctly. You will need to provide two sequential MFA codes. You get these codes from the MFA application you just set up.

NOTE: The codes must be sequential and entered in the correct order in the following command. (Replace ```123456``` and ```654321``` with codes from your app):
```bash
aws-vault --backend=kwallet exec -n ${ACCOUNT_NAME} -- aws iam enable-mfa-device --user-name ${LOGIN} --serial-number arn:aws:iam::${ACCOUNT_NUMBER}:mfa/${LOGIN} --authentication-code-1 123456 --authentication-code-2 654321
```

You need to configure your AWS CLI tools to make use of the virtual MFA device. You can either add this to your ```~/.aws/config``` file manually or run the following bash snippet:
```bash
AWS_CONFIG_FILE=~/.aws/config
cat >>${AWS_CONFIG_FILE} <<EOH

[profile ${ACCOUNT_NAME}]
output = json
region = us-west-2

[profile ${ACCOUNT_NAME}-admin]
source_profile = ${ACCOUNT_NAME}
role_arn = arn:aws:iam::${ACCOUNT_NUMBER}:role/nubis/admin/${LOGIN}
mfa_serial = arn:aws:iam::${ACCOUNT_NUMBER}:mfa/${LOGIN}

[profile ${ACCOUNT_NAME}-ro]
source_profile = ${ACCOUNT_NAME}
role_arn = arn:aws:iam::${ACCOUNT_NUMBER}:role/nubis/readonly
mfa_serial = arn:aws:iam::${ACCOUNT_NUMBER}:mfa/${LOGIN}
EOH

```

To test that everything has been set up correctly, run the following command:
```bash
aws-vault exec ${ACCOUNT}-ro -- aws ec2 describe-regions
```

If everything works correctly, you will be prompted for an MFA token. After entering it you should see some output similar to this:
```bash
Enter token for arn:aws:iam::517826968395:mfa/jcrowe: 123456
{
    "Regions": [
    ~snip~
        {
            "Endpoint": "ec2.eu-west-1.amazonaws.com", 
            "RegionName": "eu-west-1"
        }, 
    ~snip~
        {
            "Endpoint": "ec2.us-west-2.amazonaws.com", 
            "RegionName": "us-west-2"
        }
    ]
}
```

Finally you should attempt to login to the web console for the account by running this command. It should open a new tab in your browser:
```bash
aws-vault --debug login ${ACCOUNT}-ro
```

### nubis-builder
This is a collection of tools we built to drive Packer, greatly simplifying Packer configuration. It's fairly simple to install and comes with a few of its own dependencies that you need to install.

#### jq
We use [jq](https://stedolan.github.io/jq/) to munge [JSON](http://json.org/) data from within [Bash](http://www.gnu.org/software/bash/). From the [jq site](https://stedolan.github.io/jq/):
>jq is like sed for JSON data – you can use it to slice and filter and map and transform structured data with the same ease that sed, awk, grep and friends let you play with text.

You can install it by following the instructions on the [download](https://stedolan.github.io/jq/download/) page.

NOTE: You need at least version 1.4 of jq. If your package manager does not have a recent enough version you will need to install it manually following the instructions above.

For Linux users you can:
```bash
aptitude install jq
```

Homebrew users:

```bash
brew install jq
```

#### Packer
[Packer](https://www.packer.io/) (from Hashicorp) is the image building tool we use to build the Nubis system images.

Built in Go, it's a simple .zip file to [download](https://www.packer.io/downloads.html) with static binaries in it. No dependencies or installation pain. Simply follow the instruction [here](https://www.packer.io/docs/installation.html).

NOTE: You need packer version v0.8.1 or newer.

Homebrew users (requires Caskroom):
```bash
brew install caskroom/cask/brew-cask
brew install packer
```

#### Setup Path
While this step is not mandatory, it sure is convenient to have the nubis-builder tools on your path. You can do this one time by:
```bash
PATH=/path/to/your/clone/of/nubis-builder/bin:$PATH
```
You can make this automatic on login by adding it to the bottom of your ```~/.bashrc``` file:
```bash
echo "PATH=/path/to/your/clone/of/nubis-builder/bin:$PATH" >> ~/.bashrc
```
Of course in both of these examples you will need to change */path/to/your/clone/of* to the actual path on your system.

### Terraform (0.6.16+)
Get it from [Terraform.io](https://www.terraform.io/downloads.html). We use Terraform for deploying everything from the account to the application. If you are interested in the decision to use Terraform over Cloudformation you can read about it [here](./TEMPLATING.MD).

It's a simple Go binary bundle, just unzip and drop in your $PATH

Make sure you obtain at least version 0.6.16, but less than 0.7.

Try [this path](https://releases.hashicorp.com/terraform/0.6.16/).

### credstash (1.11.0+)
[Credstash](https://github.com/fugue/credstash) is a tool for managing our secrets into DynamoDB and KMS. It's a dependency we are hoping to get rid of, but for now, you'll need in your $PATH as well.

It's a Python PIP package, so assuming you have a working Python, just do

```bash
pip install "credstash>=1.11.0"
```

### Fin
That should be all you need to get started. If you run into any issue or have any trouble at all please reach out to us. We are happy to help and are quite interested in improving the project in any way we can. We are on irc.mozilla.org in #nubis-users or you can reach us on the mailing list at nubis-users[at]googlegroups.com
