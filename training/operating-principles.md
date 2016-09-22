# New Operating Principles
In the previous section we looked at some of the challenges we face in our current working model. We also looked at some ideas that we could use to improve upon our current working model. In this section we are going to focus in on cloud specific ideas and take a closer look at some agile principles and technologies.

 - [New way of thinking](#new-way-of-thinking)
 - [Twelve Factor Review](#twelve-factor-review)
 - [Agile Development](#agile-development)
 - [Symantic versioning](#symantic-versioning)
 - [Code Reviews](#code-reviews)
 - [Decentralization](#decentralization)
 - [git and GitHub](#git-and-github)
 - [System level configuration](#system-level-configuration)
  - [Packer](#packer)
  - [Puppet (masterless)](#puppet-masterless)
  - [Consul and Confd](#consul-and-confd)
 - [Image Upgrades](#image-upgrades)
 - [To Autoscale or Not to Autoscale](#to-autoscale-or-not-to-autoscale)
 - [Tainted Instances](#tainted-instances)
 - [Security Requirements](#security-requirements)

## New way of thinking (new working methodologies)
As we embark on this phase of the journey, I want to take a moment to recognize the fact that I am asking you to be open minded to some new ideas. I am going to present some concepts that may be new and possibly even uncomfortable. I encourage you to think critically about these ideas and ask questions as they come to mind. It is important that you understand the material, but it is more important that you understand the why behind it. If something does not make complete sense, please challenge the idea.

Ultimately you do not have to agree with these ideas, although I hope you do. Having said that, it is vital that you understand the concepts. We have an exciting opportunity to reinvent the way we work. In order for us to get it right and be constructive, we need to have some shared understanding. I hope we are able to create that understanding here.

I am going to start off by going over some general best-practice concepts that are widely accepted cloud methodology. The discussion will become more specific as we move along. I want to emphasize that these widely accepted ideas are not mine. I did not invent anything new here. I have simply collected together some of the learning my team has done over the past two years, in hopes that it will be valuable to you.

##  Twelve Factor Review
I have taken this directly from [12factor.net](https://12factor.net) and will only spend a minute reviewing it here.

0. Codebase
  - One codebase tracked in revision control, many deploys
0. Dependencies
  - Explicitly declare and isolate dependencies
0. Config
  - Store config in the environment
0. Backing services
  - Treat backing services as attached resources
0. Build, release, run
  - Strictly separate build and run stages
0. Processes
  - Execute the app as one or more stateless processes
0. Port binding
  - Export services via port binding
0. Concurrency
  - Scale out via the process model
0. Disposability
  - Maximize robustness with fast startup and graceful shutdown
0. Dev/prod parity
  - Keep development, staging, and production as similar as possible
0. Logs
  - Treat logs as event streams
0. Admin processes
  - Run admin/management tasks as one-off processes

##  Agile Development
Let me start this section by saying that we are not using any agile model verbatim. Most agile development models are designed primarily for software development. While we are working entirely in code, we are not, strictly speaking, building a piece of software. We are instead developing a model for standardized deployments in the cloud.

The Nubis project itself has a substantial amount of software development going on. In those cases we have adopted a stricter adherence to agile development methodologies. We align most closely with the Dynamic Systems Development Method (DSDM). For the sake of this training however, we will discuss the aspects of agile development as they relate to deploying applications, and by proxy infrastructure, in a cloud environment.

When we take a look at the opportunities that the cloud provides we see nearly endless possibilities. We have learned, in the datacenter, that if we build and configure technology without a well defined plan, we find ourselves with a system that is unnecessarily complex, difficult to maintain, and cumbersome to use.

Take our webapp puppet module for example. It was developed organically over time and at this point, is so complicated and cumbersome that only a handful of highly experienced folks even begin to understand how to use it. Besides the most basic and routine tasks, it is insanely difficult to make changes. When attempting to change one of the global settings, it is almost a given that it will break something in an unexpected way. This is due to the complexity of the module itself.

Allow me to explain. The original module was built in a hurry with little planning or foresight. This original module was lacking a number of features. As those features became necessary for new sites, custom configuration files were hacked on to accommodate. That is, the base module was not patched to add new functionality as there was a fear of breaking existing deployments. Over time this custom configuration mentality lead to a situation where most sites have some custom configuration. Therefore when making a change to the original portion of the module it is nearly impossible to comprehend how it will affect any of the dozens of custom configurations, which depend on the original module.

The original module should have been designed, from the start, with some way to patch it. That is to say, add additional functionality, in a way that included the ability to test changes. That way the module could have been maintained in a sane and logical fashion. In stead of dozens of custom configurations, which often duplicate functionality, the core module could have been extended in a way that would benefit future deployments. In order to achieve this, there must be some structure, or organization, around the way we approach development and maintenance of the things we build.

This is where agile development methodologies come into play. This is an existing framework that we can adopt to assist us in creating maintainable and extensible code from the start. These ideas provide us the opportunity to structure our work in a way that leads to easily understandable code. Code that is more feature-full while at the same time being less complex. Code that any technician can pick up, quickly understand and therefore submit patches.

The [Agile Manifesto](http://www.agilemanifesto.org/principles.html) is based on twelve principles:
 0. Customer satisfaction by early and continuous delivery of valuable software
 0. Welcome changing requirements, even in late development
 0. Working software is delivered frequently (weeks rather than months)
 0. Close, daily cooperation between business people and developers
 0. Projects are built around motivated individuals, who should be trusted
 0. Face-to-face conversation is the best form of communication
 0. Working software is the principal measure of progress
 0. Sustainable development, able to maintain a constant pace
 0. Continuous attention to technical excellence and good design
 0. Simplicity—the art of maximizing the amount of work not done—is essential
 0. Best architectures, requirements, and designs emerge from self-organizing teams
 0. Regularly, the team reflects on how to become more effective, and adjusts accordingly

<br>
There are a number of important concepts hidden in these principles:

 - We do not focus on through and complete design before we start to build. We focus on working code. We try to get prototypes up an running quickly. Through this process we discover requirements organically. We believe this is a much faster, and more through, way to gather requirements. This does not mean that we put half-baked code into production. The final product needs to be well understood and feature-full. The point here is that, while the final product needs to be well designed, we should not wait to start prototyping until we have a full set of requirements.
 - We move quickly and pivot often. It is not necessary to know every requirement from the start if we have good principles to work from. If, for example, we understand that everything should be built in a decoupled fashion, then we can build small components with confidence. We therefore understand that any particular component can be swapped out quickly and easily. Decisions then take on less importance. This allows us to focus less on the specific details, but rather on the big picture. It allows us to create alignment faster, as the team members are focused more on the outcome over the specific technology.
 - We collaborate in a rapid, high-bandwidth fashion. We do not schedule daily stand-ups, rather we have discussions over video as often as necessary. This is not only necessary for a high degree of collaboration, it also facilitates it. Through frequent, high-bandwidth communication we create understanding, reduce mistakes and increase productivity. This also reduces the need to exhaustively over document.
 - We document as little as possible. Documentation is often out of date as soon as it is written, especially in an agile environment. We expect, and require, code to be self documenting. That is, written in a way that anyone reasonably familiar with the language can understand.
 - Decisions are based on technical requirements first and foremost.
 - Team members are trusted to make sound decisions and deliver on time. The corollary to this is that team members must prove themselves trustworthy. They must deliver on time and raise any issues they are having.
 - Transparency is paramount.

##  Symantic versioning
As we discussed previously, everything should be pinned at a version. This brings with it the need to use some sort of versioning standard. I recognize that most distributions and projects have their own versioning schema. What we will discuss here is the schema that we are adopting for ourselves and our work. It is worth pointing out that most projects are using something very close to this, in fact you will probably recognize it.

We will be using [semantic versioning](http://semver.org/) without modification. This takes the form of vX.X.X, where v stands for 'v'ersion and X is a positive integer. The integers segments stand for MAJOR.MINOR.PATCH, where:

 - MAJOR
  - Release that is substantially different or is not backwards compatible
 - MINOR
  - Includes all changes not covered under MAJOR (most typical release)
 - PATCH
  - Specific to fixing regressions or emergency, security related, update

<br>
There is a -PRE_RELEASE option for development releases. Additionally there is a _BUILD_METADATA option available in cases where it is desirable to append a git hash or similar piece of identifying information.

As a side note, we use an underscore ```_``` in place of a plus ```+``` due to limitations in Amazon's IAM name spacing. This is the only place where we differ form Semantic versioning.

Example Release Numbering:
 - v1.2.0 (Normal Release)
  - v1.2.0-dev
  - v1.2.0-dev_githash
  - v1.2.0_githash
 - v1.2.1 (Patch / Security Release)
  - v1.2.1-dev
  - v1.2.1_githash
 - v1.3.0 (Normal Release)
  - v1.3.0-dev

## Code Reviews
This will be a new, and possibly uncomfortable, concept for many of you. Not to worry, code reviews are actually quite awesome. They provide a way for the team to keep up to date with what is going on. They greatly reduce errors going into production. They help to ensure code is up to par and self documenting. They are an excellent way for junior coders or new participants to learn and grow. I will avoid detail here as it will become more obvious through examples and future discussion.

If you are interested, there is a well written post on code reviews [here](https://www.atlassian.com/agile/code-reviews)

## Decentralization
There are a number of new concepts to adopt when working in the cloud. Decentralization is the idea that each and every component should be autonomous to as great an extent as possible. There should not be any cross application or cross account dependencies. As far as possible, there should not be any dependencies back to a datacenter.

In other words, an outage in a datacenter should in no way impact the ability to set-up or operate an application in the cloud. Likewise, the failure of an application in the cloud should not adversely affect any other application. Exchanges of information should be atomic and should route over public interfaces (AKA no VPNs).

If one application relies on information from another application, regardless of location, it should be able to degrade gracefully. This can be achieved through intelligent application design, through caching, through bulk updates, etcetera. The idea is that if another application is malfunctioning your application should continue to function to the extent that it can. Ideally without any noticeable degradation.

Here is a classic example. Application A connects directly to the database of application B to gather user login information. This is bad practice as the database connection can not natively be encrypted and it exposes both applications to a wide range of potential failures. Further it requires a high level of coordination between the two development teams, as well as the operational teams, any time maintenance is required on either application.

A better design is for application B to provide a secured, public API endpoint which application B can consume. This connection can route over the public internet. With a well defined protocol, application B can perform maintenance while returning valid error codes, or better yet cached data, through its API. This greatly reduces complexity and interdependency. It allows, for example, application B to restructure the database without coordination with the team in charge of application A.

The idea of decentralization carries through the entire architectural designs of every portion of a system. It is a shift in mentality that is critical to success in the cloud.

Here is another example. A single application resides in a datacenter and has a hot-standby fail-over site in the cloud. The application relies on a NFS filer in the datacenter. A poor first design had a VPN connection from the cloud back to the datacenter to access this content. This sets up many potential issues. A better design was devised where the data was synced from the datacenter to a cloud storage service. This way the cloud based deployment could function regardless of the failure in the datacenter. The original design was limited in that it only accounted for a subset of failures.

As you can see,the cloud is not a magic bullet. It will not protect you from bad design decisions. In fact, the cloud provides so much flexibility that it makes it very easy to make poor architectural decisions. 


##  git and GitHub
We use [git](https://git-scm.com/) exclusively as our VCS. We use [GitHub](https://github.com/) as our repository store. I am not going to go into great detail about how to use git or GitHub, you can find some excellent tutorials on the web. Some good ones from Atlassian on git can be found [here](https://www.atlassian.com/git/tutorials/). GitHub has created a number of guides that you can find [here](https://guides.github.com/).

What I am going to discuss here is a brief overview of how how specifically we use these tools. The basic work-flow looks like:

fork -> clone -> branch -> Make Changes -> Test -> add -> commit -> push -> pull-request -> Code review & Comment -> merge

Lets take a look at each of these steps in more detail.

 - fork
  - The first step it to fork the primary repository. This gives you your own private code from which to work. This is done on GitHub.
 - clone
  - This step is done with the git client on your local system. This downloads a copy of the code locally.
 - branch
  - You should make it a habit to always work on feature branches. This gives good separation to the features you are working on and provides an easy mechanism for you to check in code in an isolated manner.
 - Make Changes
  - This is where the magic happens. There is nothing special here, you just code to your hearts content.
 - Test
  - This step seems obvious at first glance. Only check in working code. There is a subtlety here however. All commits to a project should be atomic. In other words you should not be making commits to an upstream repository that are half finished. All upstream commits should be fully working and tested code.
 - add
  - Simply add each individual change, typically file, that you wish to include in this commit. Unlike subversion this step does not push out any code. It simply allows you to be pedantic and specific about which changes you intend to include.
 - commit
  - Here we are basically just adding a commit message. You should never include a commit message on the command line, rather allow the editor to open up. This provides you with one final chance to review what is being shipped with this commit and, just as importantly, what is not being shipped in this commit.
 - push
  - Finally, you push the code off of your local system. This will push the changes you have added and committed up to your fork on GitHub.
 - pull-request
  - This step generates a diff for you and creates the official request for your code to be accepted into the primary repository. This is one of the places where GitHub has made working with git extremely easy and is one of the largest reasons that we recommend using GitHub over other git orchestration systems.
 - Code review & Comment
  - At this point the reviewer takes over the process. They will review the code and ask any clarifying questions. The reviewer ensures that the code looks operational and fits within the basic standards.
  - It is not expected that the reviewer actually builds and tests the code, we differ from many projects on this point. We trust that you have validated the code and that you are the best person to do so.
  - I will mention that if you gain the reputation of someone who often checks in broken code, this process becomes more cumbersome as the reviewer will tend to ask more questions and scrutinize the code more closely. You should do everyone a favor and test your code before making a pull-request.
  - At a minimum, the reviewer must comment "r+" to note that a review has been accomplished.
 - merge
  - Easy as pie. Apply the diff to the primary repository. Yet another place where GitHub makes things super simple, and another point worth recommending GitHub.

This work-flow encompasses many aspects of the working methodologies we have been discussing. You should follow this process exactly and not vary until you have an excellent working knowledge of git and a solid reason for doing so. There are as many ways of using git as there are stars in the sky. This process has been developed through experience and, if followed, will help you avoid a number of pitfalls.

##  System level configuration
We are going to start to combine a number of the concepts we have been discussing. We must keep in mind the idea of immutable instances. If you recall, we discussed creating immutable instances with run-time tunables. In this section I am going to discuss both how we create the immutable instance images as well as how we adjust the run-time tunables.

 - [Packer](#packer)
 - [Puppet (masterless)](#puppet-masterless)
 - [Consul and Confd](#consul-and-confd)

### Packer
[Packer](https://www.packer.io/) is a tool used to create machine images from a simple configuration. In its simplest form it executes a number of shell commands which configure an image. The image is generally some pre-baked distribution image. This image is started up in a sandbox and the commands are run against it. Once complete the image is snap-shotted, creating a new image.

### Puppet (masterless)
We have chosen [puppet](https://puppet.com/) for configuring hosts. Of the two primary choices, Puppet and Chef, we felt there was more experience and therefore less of a learning curve by continuing to use puppet. There is one substantial difference from how we use puppet in the datacenter. We do not use puppet-masters.

Keeping in line with the concept of decentralization we discussed previously it becomes obvious that puppet-masters create dependencies. Further it breaks the basic idea of immutable instances. It is still necessary to be able to configure systems and using puppet as a standalone tool works quite nicely.

Puppet masterless is fired off during image build time by packer. This process is documented [here](https://www.packer.io/docs/provisioners/puppet-masterless.html)

### Consul and Confd
[Consul](https://www.consul.io/) is a tool that is used to provide a number of features. First it provides a key / value store that can be used to store runtime tunables. Next it provides a service discovery mechanism. Finally it provides a distributed locking service.

Run-time tunables are stored in the Consul key-value store. This provides us the ability to create images that have no secrets and are adaptable at run-time. All of the images we create contain no secrets and are publicly available. We store these secrets; api keys, database passwords, etc in this key /value store.

Service discovery is a really cool concept that makes working in the cloud much simpler. In an environment where instances are disposable, it can be difficult to figure out what the id or hostname of the instance you need to work on is. Service discovery makes this a breeze. FOr example, the Dpaste app, which we use for all of our examples, can be located by sshing to dpaste.service.consul. No need to know the ID or IP or hostname, etc. Technically the dpaste web servers register themselves with consul as providing the dpaste service.

The distributed locking service is best described with a cron example. Say you have a cron job that you would like to run each hour. If we install that cron job on our web server it works as long as there is only one server. When autoscaling increases our web server pool, this cron jub will be executed multiple times at the top of each hour. This can lead to any number of issues. If we use the consul locking service, we simply wrap our cron job in a consul lock and the first node to acquire the lock runs.

[Confd](http://www.confd.io/) is a tool that is installed on every system. It watches a portion of the consul key / value store and updates the local system in near-realtime. This is the mechanism we use to say, create a local.py file with the database connection details.

Here is another example of using the consul key / value store in combination with confd for managing run-time tunables. A firewall installed on a NAT instance blocks all outbound SMTP (outbound mail) traffic. Adding the SMTP port as a value to the appropriate key in Consul results in that port being opened on the NAT firewall.

##  Image Upgrades
As we are working with immutable instances, upgrades work a bit differently. If you recall, every package should be pinned to a specific version. This is typically done in the puppet configuration. To update a system, you simply update the version numbers in the puppet configuration and rebuild the image. System level updates are typically done by consuming a more up-to-date base image. We will see more specifics in the next section and an example a bit later on.

##  To Autoscale or Not to Autoscale
Sure, the Shakespearian reference may be a bit much but the point still holds. Autoscale everything. The basic use case for autoscaling is to horizontally scale out when demand goes up and vise-a-versa when demand drops. There is another use for autoscaling. It provides the ability to have persistent single instances in the cloud. That is to say that if you start your system in an autoscaling group of one, if the instance dies a fresh image will be started in its place.

This is quite handy when developing as you simply shutdown your test instance to get a clean image. It is also handy for hosting small applications that are not designed in a way that allows for multiple concurrent copies. Best of all, there is no downside to running in an autoscaling group all the time.

This practice makes updates more streamlined as well as the autocaaling system can boot a new updated image prior to destroying the old instance. Making for a nearly zero downtime re-deployment path for these simple applications.

## Tainted Instances
This brings us right into tainted instances. If for any reason a running instance is modified it should be considered tainted. It can no longer be relied upon to conform to the tested golden image.

While this creates quite a headache in the datacenter with lengthy auditing and compliance requirements, the cloud makes this super easy. Since everything is operating in an autoscaling group, all we need to do is kill the tainted instance and a pristine image will replace it. Simple, no?

##  Security Requirements
Operating in the cloud brings with it a few new security implications. These mostly revolve around the distributed nature of systems and differences in physical access and proximity. We must chose a cloud provider that has a good reputation for physical security (actually this is an entire discussion which includes; security, uptime, location, failure modes, and so on). If we assume that we have done our due diligence and chosen a trustworthy cloud provider, we are left with, more or less, the same concerns we have in the datacenter.

These concerns encompass a wide range of topics like:
 - Network security
 - User Management (including removal)
 - Password policies
 - Segmented privilege levels (least access)
 - Log integrity
 - Intrusion detection
 - Data Ex-filtration
 - System and package updates
 - Zero Day vulnerability patching
 - Long lived access credentials
 - System access
 - Monitoring
 - Auditing

And the list goes on... If you have ever worked with a security team, you know that they love to dream up nightmare scenarios and want every system patched against these dystopian fantasies. I jest, but the point is that there are a lot of requirements that must be accounted for with each deployment. these requirements are not diminished in the cloud, if anything they become more complex as system creep across various IAAS, PAAS and SAAS providers.

##  Wrap
We have covered a lot of material that may be unfamiliar and possibly even uncomfortable. Lets take a little break now. When we return we will take a look at how we are applying these principles to solve some rather complex issues.

