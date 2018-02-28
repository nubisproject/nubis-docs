# Getting Started

Before you can contribute to the Nubis project, you'll need to have a few
tools installed and configured.

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

As all of our code is on GitHub, you will need a git client of some kind. Most
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

## AWS Credentials

In order to work with AWS you will need to set up your credentials. All access
to AWS requires utilizing multi-factor authentication (MFA). When your user is
added to an account you will receive an encrypted email containing a key pair.

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

This should return something like `v3.7.1`.

## nubis-ctl

This is the tool that you will use anytime you need to interact with AWS
directly. It handles everything from AMI builds to Terraform deployments. In
fact, we in Nubis land use this tool for account deployments as well. This tool
primarily executes Docker containers, which basically wrap other tools and their
dependencies. Enough chit-chat, lets keep going.

### Install docker

As nubis-ctl leverages Docker containers, the first thing you will need to do is
Install Docker (obvious, I know :).

#### Linux

**NOTE:** These packages are not required, but Docker will have reduced
performance if they are not installed.

```bash

sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual

```

Install docker (this step is required):

```bash

sudo apt install docker.io

```

This section can be skipped, however you will need to prepend 'sudo' to all of
your docker commands if you do.

```bash

sudo groupadd docker
sudo usermod -aG docker $USER

```

Log out & back in to pick up your new group membership.

#### Mac

[Get it here](https://store.docker.com/editions/community/docker-ce-desktop-mac)

### Test that Docker is set up and running correctly

Try running the hello-world docker image. This will download and run the docker
image hello-world:

```bash

docker run hello-world

```

### Set up the account credentials

Using the AWS account credentials we talked about earlier, go ahead and set them
up in aws-vault using the nubis-ctl tool. It will prompt you for all necessary
input:

```bash

nubis-ctl add-account

``

### Build an AMI

Before we can build an AMI you will need some code to work with. Lets clone the
nubis-skel repository:

```bash

git clone https://github.com/nubisproject/nubis-skel.git
cd nubis-skel

```

Before we build you will need to edit the `project.json` file and change
`project_name` to something unique:

```bash

vim nubis/builder/project.json

```

Go ahead and build the AMI. This will output an AMI ID that we will need in the
deploy section below:

```bash

nubis-ctl build

```

### Deploy the project

Create a terraform.tfvars from the terraform.tfvars-dist and edit the variables.
Be sure to replace all of the `<variables>` whith appropriate ones for your
application:

```bash

cp nubis/terraform/terraform.tfvars-dist nubis/terraform/terraform.tfvars
vim nubis/terraform/terraform.tfvars

```

- account = "nubis-training"
- region = "us-west-2"
- environment = "stage"
- service_name = "`<username>`-skel"
- ssh_key_name = "`<username>`-key"
- ssh_key_file = "`<path to your ssh key>`~/.ssh/id_rsa.pub"
- nubis_sudo_groups = "`<value provided by Nubis team>`"
- nubis_user_groups = "`<value provided by Nubis team>`"

NB: The service_name and ssh_key_name **must** be unique in the AWS account.

You are finally ready to deploy nubis-skel into the AWS account. Start by
running a plan to make sure the app will deploy without errors. Then apply the
plan, this will deploy the nubis-skel application into the AWS account.

```bash

nubis-ctl deploy plan
nubis-ctl deploy apply

```

Test your deployment by navigating to the ELB endpoint that is in the outputs
section. It should look something like this:

 ```bash

 Outputs:

 address = https://www.<app-name>.stage.<region>.<account-name>.nubis.allizom.org/
 Outputs:

 ```

To finish up, remove your application from the training account. This saves us
quite a bundle of cash:

```bash

nubis-ctl deploy destroy

```

## Fin

That should be all you need to get started. If you run into any issue or have
any trouble, please reach out to us. We are happy to help and are quite
interested in improving the project in any way we can. We are on Mozilla Slack
in #nubis-users or you can reach us on the mailing list at
nubis-users[at]googlegroups.com
