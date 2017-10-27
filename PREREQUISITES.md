# Prerequisites

Before you can contribute to the Nubis project, you'll need to have a few
tools installed and configured.

## Configure shell

When your user is added to an account you will receive an encrypted email
containing The necessary information for you to access the account.

The first thing I like to do is set up some local shell variables to make the
following steps a bit simpler. Of course you will **need to replace** the
`ACCOUNT_NAME`, `ACCOUNT_NUMBER` and `LOGIN` with the ones you
received in the user credentials email.

```bash

ACCOUNT_NAME='nubis-training'; ACCOUNT_NUMBER='517826968395'; LOGIN='<login>'

```

## GitHub Account

Nubis is entirely hosted on github, and we derive most of our work-flows from
GitHub's recommended practices.

If you are new to git or GitHub, you are probably better off familiarizing
 yourself with that first. There are a lot of handy tutorials straight from the
 source [here](https://www.atlassian.com/git/tutorials/)

You will need to set up an account on GitHub. It doesn't have to be anything
special, any old account will do. To set up a GitHub account, click [here](https://github.com/join).

You will also want to set up your ssh keys with GitHub. You can do that [here](https://github.com/settings/ssh).

## git

All our source-control is in Git, so you'll need a git client of some kind. Most
distributions include a git client that you can install with your package
manager. You can also get git directly from their [downloads site](https://git-scm.com/downloads).

For apt users:

```bash

aptitude install git

```

Homebrew users:

```bash

brew install git

```

## AWS CLI

Next, you need to install the AWS CLI tool. You can install it by following the
instructions at the top of [this page](http://aws.amazon.com/cli/).

For Linux (and some mac) users:

```bash

pip install awscli

```

Homebrew users:

```bash

brew install awscli

```

## AWS Credentials

In order to work with AWS you will need to set up some credentials. This is a
somewhat involved process as all access to AWS requires utilizing a multi-factor
authentication (MFA) device. When your user is added to an account you will
receive an encrypted email containing a key pair.

**NOTE:** These keys need to remain secret. You need to take the utmost care;
**DO NOT** check them into git, send them via unencrypted email, copy them into
a pastebin, etcetera.

### aws-vault

[aws-vault](https://github.com/99designs/aws-vault) is a tool to securely manage
AWS API credentials. You will need to download this tool and place it on your
path.

Once installed you will use the aws-vault tool to authenticate for all access
and actions within AWS.

Lets start by making sure aws-vault is installed and working correctly:

```bash

aws-vault --version

```

This should return something like ```v3.7.1```.

If you are using linux you need to set your backend to use kwallet. I recommend
placing this in one of your startup scripts, say `~/.bashrc`:

```bash

export AWS_VAULT_BACKEND=kwallet

```

Now you can run the aws-vault command to set up the account. This will ask you
for the 'Access Key ID' and the 'Secret Access Key'. You will need to get those
from the user credentials email as well:

```bash

aws-vault add ${ACCOUNT_NAME}

```

It should look something like this (keys redacted):

```bash

Enter Access Key ID: AKXXXXXXXXXXXXXXXXXX
Enter Secret Access Key: jF6EfXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Added credentials to profile "nubis-training" in vault

```

Now it is time to create your virtual MFA device:

```bash

aws-vault exec -n ${ACCOUNT_NAME} -- \
aws iam create-virtual-mfa-device \
--virtual-mfa-device-name ${LOGIN} \
--outfile ${LOGIN}.png \
--bootstrap-method QRCodePNG

```

You should see output similar to the following. The number here should
correspond to the account number and the `<login>` portion of the serial
number should be your user-name:

```bash

{
    "VirtualMFADevice": {
        "SerialNumber": "arn:aws:iam::517826968395:mfa/<login>"
    }
}

```

You will need to view the ${LOGIN}.png file and use it to configure your MFA
application. If you have imagemagic installed you can try `display $LOGIN.png`
or just open it with any image viewer. I use the [duo mobile app](https://duo.com/solutions/features/two-factor-authentication-methods/duo-mobile),
however the [google authenticator app](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=en)
works as well. Really, most MFA apps work here so if you have one you already
use or prefer it should be just fine.

To finish up, you need to enable the mfa device. This step is basically proving
that you are you and everything is configured correctly. You will need to
provide two sequential MFA codes. You get these codes from the MFA application
you just set up.

**NOTE:** The codes must be sequential and entered in the correct order in the
following command. (Replace `<code 1>` and `<code 2>` with codes from
your mfa app):

```bash

aws-vault exec -n ${ACCOUNT_NAME} -- \
aws iam enable-mfa-device \
--user-name ${LOGIN} \
--serial-number arn:aws:iam::${ACCOUNT_NUMBER}:mfa/${LOGIN} \
--authentication-code-1 <code 1> \
--authentication-code-2 <code 2>

```

You need to configure the AWS CLI tools to make use of the virtual MFA device.
You can either add this to your `~/.aws/config` file manually or run the
following bash snippet:

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

vault-exec aws ec2 describe-regions

```

If everything works correctly, you will be prompted for an MFA token. After
entering it you should see some output similar to this:

```bash

Enter token for arn:aws:iam::517826968395:mfa/<login>: <code>
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

Finally you should attempt to login to the web console for the account by
running this command. It should open a new tab in your browser:

```bash

aws-vault --debug login ${ACCOUNT_NAME}-ro

```

## nubis-builder

This is a Docker image we built to drive Packer. The embedded tools greatly
simplify Packer configuration. It's quite simple to install and comes with all
of the required dependencies.

### Install docker and required libraries

#### Linux

**NOTE:** These packages are not required, but Docker will have reduced
performance if they are not installed.

```bash

sudo apt-get install \
     linux-image-extra-$(uname -r) \
     linux-image-extra-virtual

```

Install docker:

```bash

sudo apt install docker.io

```

This section can be ignored, however you will need to perpend 'sudo' to all
docker commands (including adjusting the bash alias above).

```bash

sudo groupadd docker
sudo usermod -aG docker $USER

```

Log out & back in to pick up your new group membership.

#### Mac

[Get it here](https://store.docker.com/editions/community/docker-ce-desktop-mac)

### Test that Docker is set up and running correctly

Try running the hello-world docker image. This will download and run the docker
image:

```bash

docker run hello-world

```

### Run the nubis-builder Docker image

Lets start by setting up a few shell alias to make these commands a bit more
manageable:

```bash

alias vault-exec="aws-vault exec ${ACCOUNT_NAME}-admin --"

NUBIS_DOCKER=( 'docker' 'run' \
                '-u' "$UID:$(id -g)" \
                '--interactive' \
                '--tty' \
                '--env-file' "$(echo ~)/.docker_env" \
                '-v' "$PWD:/nubis/data" )

```

Clone the nubis-skel repository:

```bash

git clone git@github.com:nubisproject/nubis-skel.git
cd nubis-skel

```

Set up a docker variables file:

```bash

DOCKER_CONFIG_FILE=~/.docker_env
cat >>${DOCKER_CONFIG_FILE} <<EOH
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_SESSION_TOKEN
AWS_SECURITY_TOKEN
EOH

```

Fire off the nubis-builder docker image. This will create an AWS AMI for you to
deploy:

```bash

vault-exec "${NUBIS_DOCKER[@]}" -e GIT_COMMIT_SHA=$(git rev-parse HEAD) nubisproject/nubis-builder:v0.5.0

```

Create a terraform.tfvars from terraform.tfvars-dist and change values to these
settings (Be sure to replace all of the `<variables>` whit appropriate ones
    for your application):
 - account = "nubis-training"
 - region = "us-west-2"
 - environment = "stage"
 - service_name = "```<username>```-skel"
 - ssh_key_name = "```<username>```-key"
 - ssh_key_file = "/```<path to your ssh key>```/.ssh/id_rsa.pub"
 - nubis_sudo_groups = "```<value provided by Nubis team>```"
 - nubis_user_groups = "```<value provided by Nubis team>```"

 The service_name and ssh_key_name **must** be unique in the Nubis account.

```bash

cp nubis/terraform/terraform.tfvars-dist nubis/terraform/terraform.tfvars
vim nubis/terraform/terraform.tfvars

```

Deploy nubis-skel to the training account. Start by executing a plan to make
sure of the changes you will be making. Next apply the plan, this will deploy
the nubis-skel application into the account.

```bash

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.3.0 plan
vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.3.0 apply

```

 Test your deployment by navigating to the ELB endpoint that is in the outputs
 section. It should look something like this:

 ```bash

 Outputs:

 address = https://www.<app-name>.stage.<region>.<account-name>.nubis.allizom.org/
 Outputs:

 ```

To finish up, remove your application from the training account:

```bash

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.3.0 destroy

```

## Fin

That should be all you need to get started. If you run into any issue or have
any trouble, please reach out to us. We are happy to help and are quite
interested in improving the project in any way we can. We are on Slack
in #nubis-users or you can reach us on the mailing list at
nubis-users[at]googlegroups.com
