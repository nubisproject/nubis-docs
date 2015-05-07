## Project Onboarding

This document is hear to guide us through the process of on-boarding your application in AWS using the [Nubis](https://github.com/Nubisproject) project.

### Checklist
The steps we will take during this process are:
 1. You [gather information](PROJECT_ONBOARDING.md#gather-information)
 1. We [create your AWS account](PROJECT_ONBOARDING.md#create-account) in the Sandbox
 1. You [generate your AWS API keys](PROJECT_ONBOARDING.md#generate-api-keys)
 1. Everyone [meets](PROJECT_ONBOARDING.md#meeting-time) to discuss architectural requirements / design
 1. You [build out](PROJECT_ONBOARDING.md#application-build-out) your application in the Sandbox
 1. You initiate the [promotion to Dev](PROJECT_ONBOARDING.md#promote-to-dev) process
 1. You initiate the [promotion to Prod](PROJECT_ONBOARDING.md#promote-to-prod) process

### Gather Information
We need to know a few pieces of information about your application. This is used to track resources, for troubleshooting and for billing purposes. If you do not have all of this information, do not worry, we can help you figure it out. Once you have gathered all of this information you simply fill out [this form](link to Bugzilla form) to kick off the rest of the process.

> Link to Bugzilla form (or list here the instructions for them to send the information to us). I am suggesting we create a Bugzilla form with inputs specifically for this information. When the user fills out this form it will both initiate the on-boarding process as well as provide us with the information we need to get started. I suggest two additional Bugzilla forms later in this document.

 The information we need from you is:
 1. Name of your project (AKA the "Service Name") [found here] (https://inventory.mozilla.org/en-US/core/service/)
 1. Email address of the ["Technical Owner"](link to email address requirements)
 1. List of people who should attend the kickoff Meeting

### Create Account
As soon as you submit the above information to us we will create your AWS account.
 > Will we use the Technical Owner email address for this? If not provide details here about how we will provide the information to them.

Once we have done that we will notify you by...
> (how?).

### Generate API Keys
Once you are logged into your new account you will need to generate an API key pair by following the instructions [here](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSGettingStartedGuide/AWSCredentials.html). You will use this key pair to deploy resources (such as EC2 instances) into the sandbox. You should keep in mind that these keys are [secret](https://github.com/Nubisproject/nubis-docs/blob/master/SECURITY.md) and should not be shared with anyone.

### Meeting Time
We will schedule a kickoff meeting with all the necessary folks so we can all sit down and determine how we can help you to succeed. In this meeting we will discuss; design requirements, architectural needs, best practices and so on. Not to worry, we have a presentation all set up and will try to make this as painless as possible for you and your team.

### Application Build Out
Now that we have a design it is time to build the resources necessary to support your application. To assist you with your application build out we have prepared a number of documents.

 * First you will want to check out our [System Overview](https://github.com/Nubisproject/nubis-docs/blob/master/SYSTEM_OVERVIEW.md) document. It will give you a general sense of how all the pieces fit together.
 * Then I recommend you take a peek at our [Git & GitHub](https://github.com/Nubisproject/nubis-docs/blob/master/GIT_GITHUB.md) doc. This will aid you in setting up your repository for deploying with the Nubis Project.
 * [Prerequisites](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md) will get you set up with all the necessary tools.
 * Next you should peruse our [Nubis Builder](https://github.com/Nubisproject/nubis-builder/blob/master/README.md) document which covers building AMIs using [Packer](https://www.packer.io/) and [Puppet](https://puppetlabs.com/).
 * Finally you should take a gander at out [CloudFormation](https://github.com/Nubisproject/nubis-docs/blob/master/CLOUDFORMATION.md) document which covers things like using nested stacks to simplify your CloudFormation templates.

<br>
Armed with this informtation you can start to craft a CloudFormation template for your application. For an example check out the nubis-mediawiki project [template](https://github.com/Nubisproject/nubis-mediawiki/blob/master/nubis/cloudformation/main.json).
 
### Promote to Dev
The next step on the road to getting your application into production is to initiate the process to get it deployed into Dev. This should be a super simple process as long as you followed the best practices mentioned above. If so, you simply fill out [this little form](link to another Bugzilla form for promotion process) and we will do the rest. We will be setting up a Continuous Integration (CI) system to deploy your project into Dev. This CI system will deploy your application using the exact same CloudFormation templates that you use to deploy in the Sandbox. The process will go something like [this](https://mana.mozilla.org/wiki/display/EA/Environment+promotion).

### Promote to Prod
Once your application is running in Dev it is time for you to do your User Acceptance Testing (UAT). Once you have completed your UAT and you are ready to promote your application into production, simply fill out this [form](link to Bugzilla form for production promotion). We will then set up the CI system to deploy your application into Production. We will also schedule the following meetings
> what meetings go here? make list CAB, what else?

During these meetings we will work with you to schedule the actual go-live event. This typically includes things like coordinating with the Mozilla Operations Center (MOC) and scheduling final content sync and DNS cut-over.

### Winning
That is all there is to it. If you have any feedback on this document, this process, or anything Nubis Project related please feel free to drop us a line. We are in #infra-aws on [irc](irc.mozilla.org) or you can shoot us an [email](mailto:infra-aws@mozilla.org). Happy clouding.