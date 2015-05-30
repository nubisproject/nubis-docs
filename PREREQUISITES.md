## Prerequisites
Before you can contribute to the Nubis project, you'll need to have a set of tools installed..

### GitHub Account
Nubis is entirely hosted on github, and we derive most of our workflows from GitHub's recommended practices.

If you are new to git or GitHub, you are probably better familiarizing yourself with that first.

You don't strictly speaking need a GitHub account, but if you plan on contributing back, you'll need one. It doesn't have to be special, just any old account will do. To set up for a GitHub account, click [here](https://github.com/join).

### git
All our source-control is in Git, so you'll need a git client of some kind. Most distributions include a git client that you can install with your package manager. You can also get git directly from their [downloads site](https://git-scm.com/downloads).

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

This tool is a nubis-builder dependency, which means you will need it to build images. However it is quite handy on its own as an alternative to using the AWS Web Console.

At this point you should test this tool like so:
```bash
aws cloudformation list-stacks
```

If you get a JSON formated list of stacks back, congratulations, you can move on. If you got an error, you will need to do a bit of troubleshooting. Some of the most common issues tend to be conflicting [boto](https://github.com/boto/boto) configurations or old aws-cfn-tools configured (check for an ~/.aws/ec2.env file). If you get an authentication error, go back and confirm you have correctly set up your [AWS Credentials](#aws-credentials).

### nubis-builder
This is a collection of tools we built to drive Packer, greatly simplifying Packer configuration. It's fairly simple to install and comes with a few of its own dependencies that you will need to install.

You will need to follow the quick start instructions from the nubis-builder project [here](https://github.com/Nubisproject/nubis-builder#builder-quick-start). You do not need to go into the *New project quick start* section at this point.
