## Getting started with the Nubis Project
Welcome to the Nubis Project. We hope you will find that it meats your requirements and is easy to use. In this document I will introduce you to the Nubis Project and give you a number of links to other documents that will help you along.

The Nubis Project is in essence a framework for deploying applications to the cloud. At this time we support only Amazon Web Services (AWS). For an overview of our design principles I recommend you read out [manifesto](https://github.com/Nubisproject/nubis-docs/blob/master/MANIFESTO.md).

### Familiarize yourself with the Nubis Project
Now, to get you up to speed with everything you will need to know to use the Nubis Project, I will provide for you a reading list. Not to worry, while this list looks long most of the documents are quite short.
* [Nubis Overview](WRITE_ME) Will give you an outline of the pieces of the project.
* [Git & GitHub](https://github.com/Nubisproject/nubis-docs/blob/master/Git_GitHub.md) provides some advice specific to Nubis.
* [CloudFormation](https://github.com/Nubisproject/nubis-docs/blob/master/CloudFormation.md) walks through some recomendations on structure and workflow.
* [Project Onbording](https://github.com/Nubisproject/nubis-docs/blob/master/Project_Onbording.md) will guide you through onboarding your first application.

### Deployment
Now that you are familiar with the project and the process it is time to get coding. The first step is to assemble your deployment repository. Then it will be time to deploy into the sandbox.

As we have seen in various examples through these documents, you will need to create a deployment repository. Take a look at the [System Overview](link) document for details.

Once your repository is all set up the next step it to deploy into the sandbox. You can deploy bu following the procedures outlined in the [Project Onbording](https://github.com/Nubisproject/nubis-docs/blob/master/Project_Onbording.md#Application Build Out) doc. Some example commands can be found in our trusty [nubis-mediawiki](https://github.com/Nubisproject/nubis-mediawiki/blob/master/nubis/cloudformation/README.md) repository.


### Bugs, Contributions and more
We are super excited to have you hear. If you have stumbled into an issue there are several ways to address it.

First, you can fix the issue yourself and file a pull request. You will find a guild in our [Contributing Doc]((https://github.com/Nubisproject/nubis-docs/blob/master/CONTRIBUTING.md).

Next, you can file an issue. Simply navigate to the Nubis Project space on Github [here](https://github.com/Nubisproject), select the appropriate repository and click on the issues link. For example to file an issue against nubis-stacks you would go [here](https://github.com/Nubisproject/nubis-stacks/issues)

Finally if you are looking for a new feature to be supported, simply follow the [Feature Requests](https://github.com/Nubisproject/nubis-docs/blob/master/FEATURE_REQUESTS.md) guide.

---
## TODO
Document these things
* set up git repo
 * add nubis directory
* link to structure doc
 * discuss packer and nubis-builder
 * discuss packers use of puppet
* describe cloudformation template system
 * link to cloudformation layout doc?
* discuss what is and is not appropritae to place in the bin directory
* walk through deployment of application
* need to link to set up for Nubis doc (set up aws, git, github, etc...)