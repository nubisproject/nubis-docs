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

### AWS Credentials
Presently, we support only Amazon Web Services as a build platform, even though Packer itself supports many more. This means you'll need valid AWS API keys to build images. You should have received these when you got your account credentials. If not you can generate them yourself by following the instructions [here](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html#Using_CreateAccessKey).

NOTE: These keys need to remain secret. You need to take the utmost care; DO NOT check them into git, send them via unencrypted email, copy them into a pastebin, etcetera.

You will want to place these credentials in several places, which will be pointed out as we go along. For now just put them in ~/.aws/credentials like so:

```bash
[default]
aws_access_key_id = XXX
aws_secret_access_key = XXX
```

### AWS SSH Key Pair
You will need to either [create](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) or [import](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#how-to-generate-your-own-key-and-import-it-to-aws) an ssh key to AWS. This key will be provisioned on any EC2 instances you create so you can login for troubleshooting or other necessities.

You will also need to contact XXX to have your ssh key placed on the jump host.
> WHO DO WE CONTACT???


### AWS CLI
Next, you need to install the AWS CLI tool. You can install it by following the instructions at the top of [this page](http://aws.amazon.com/cli/). For Mac and Linux users you can simply:
```bash
pip install awscli
```

Homebrew users:

```bash
brew install awscli
```

This tool is a nubis-builder dependency, which means you will need it to build images. However it is quite handy on its own as an alternative to using the AWS Web Console.

At this point you should test this tool like so:
```bash
aws cloudformation list-stacks
```

If you get a JSON formated list of stacks back, congratulations, you can move on. If you got an error, you will need to do a bit of troubleshooting. Some of the most common issues tend to be conflicting [boto](https://github.com/boto/boto) configurations or old aws-cfn-tools configured (check for an ~/.aws/ec2.env file). If you get an authentication error, go back and confirm you have correctly set up your [AWS Credentials](#aws-credentials).

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
You can make this automatic on login by adding it to the bottom of your bashrc file:
```bash
echo "PATH=/path/to/your/clone/of/nubis-builder/bin:$PATH" >> ~/.bashrc
```
Of course in both of these examples you will need to change */path/to/your/clone/of* to the actual path on your system.

### Fin
That should be all you need to get started. If you run into any issue or have any trouble at all please reach out to us. We are happy to help and are quite interested in improving the project in any way we can. We are on irc.mozilla.org in #nubis-users or you can reach us on the mailing list at nubis-users[at]googlegroups.com
