## Prerequisites

Before you can contribute to the Nubis project, you'll need to make sure of a few things beforehand.

Nubis is entirely hosted on github, and we derive most of our workflows from GitHub's recommended practices.

If you are new to git or GitHub, you are probably better familiarizing yourself with that first.

### GitHub Account

You don't strictly speaking need a GitHub account, but if you plan on contributing back, you'll need one. It doesn't have to be special, just any plain account will do just fine.

### git

All our source-control is in Git, so you'll need a git client of some kind.

### AWS Credentials

Presently, we support only Amazon Web Services as the build platform, even though Packer itself supports many more. This means you'll need valid AWS credentials to build images. It will come in handy to test them as well.

### packer (http://packer.io)

Packer (from Hashicorp) is the image building tool we use to build the Nubis system images. It's a necessary tool if you want to be able to build images yourself, or test that your changes don't break the build process.

Built in Go, it's a simple .zip file to download with static binaries in it. No dependencies or installation pain. Just follow the instruction on the Packer website.

### nubis-builder

This is a collection of tools we built to drive Packer, greatly simplifying Packer configuration.

It's fairly simple to install and comes with some dependencies.

Just follow the instructions from https://github.com/Nubisproject/nubis-builder

Once done, for convenience, just put it in your $PATH

### AWS CLI

It's a nubis-builder dependency, so you'll need it. But it's also handy on its own as an alternative to using the AWS Web Console.
