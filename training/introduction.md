# Introduction
I am going to say some things today that are going to upset some people. I want to say right up front that I acknowledge that some of you have a lot invested in how we currently operate. Many of you have worked for years within the current datacenter paradigm. My intention is not to lambaste anyone. The intent of the discussion today is to discover some challenges we might all be facing and talk about mitigating those challenges. Ultimately our job in IT is about creating experiences that serve our customers needs while creating an environment that they enjoy working in. I believe that we are facing a crisis in IT today. Our customers are leaving in droves to other offerings that provide better experiences. This is completely understandable from a customer perspective. However, it is my opinion that we, collectively have the knowledge and experience to provide what our users need in a way that will not only serve their needs, but that they enjoy, is reliable and secure.

We have skills, expertise and experience in this organization that can rival any other shop out there. We simply need to take a look at some of the user experiences we are currently providing and see how we can improve on them. Some of the ideas you are going to hear today might sound a bit radical, they might be difficult to accept. I ask however, that you keep an open mind and try to look past the how, to the why. What I am going to present here is not perfect, it is a work in progress. Some of the decisions have been made, some of the ideas are central, however we have a long ways to go before we achieve success. That is where all of you come in, we need to come together as an organization and work together to try and provide the absolute best user experience we can.

 - [On listening to our customers](#on-listening-to-our-customers)
 - [Examples of current state](#examples-of-current-state)
 - [How can we Improve](#how-can-we-improve)
 - [Current operating model](#current-operating-model)
  -  [Illustration of current challenges](#illustration-of-current-challenges)
  - [List of issues in our current work-flows](#list-of-issues-in-our-current-work-flows)
     - [Tainted resources](#tainted-resources)
     - [Lack of package pinning](#lack-of-package-pinning)
     - [Untested changes to production systems](#untested-changes-to-production-systems)
     - [Lack of isolation between applications](#lack-of-isolation-between-applications)
     - [Too many ways a system can be mutated](#too-many-ways-a-system-can-be-mutated)
     - [Puppetmasters ensure eventual consistency](#puppetmasters-ensure-eventual-consistency)
     - [Puppet's inability to guarantee symmetry among systems](#puppets-inability-to-guarantee-symmetry-among-systems)
     - [A word on backups](#a-word-on-backups)
 - [User experiences](#user-experiences)
 - [Future Operating Model](#future-operating-model)
  - [Specific areas of improvement](#specific-areas-of-improvement)
     - [Automate all the things](#automate-all-the-things)
     - [Built on cloud technology](#built-on-cloud-technology)
     - [Provide self service opportunities](#provide-self-service-opportunities)
     - [Create standards](#create-standards)
     - [Treat datacenters as reusable components](#treat-datacenters-as-reusable-components)
     - [Exterminate the "Human API"](#exterminate-the-human-api)
     - [Use more community resources](#use-more-community-resources)
     - [Revision everything](#revision-everything)
     - [Transition work-flow to GitHub](#transition-work-flow-to-github)
     - [Code Reviews](#code-reviews)
     - [Provide Application isolation](#provide-application-isolation)
     - [Provide a platform that can autoscale](#provide-a-platform-that-can-autoscale)
     - [Bit for bit repeatable deployments](#bit-for-bit-repeatable-deployments)
     - [Destroy Tainted resources](#destroy-tainted-resources)
     - [Reduce time required to stand up a new application](#reduce-time-required-to-stand-up-a-new-application)
     - [Provide analytical and trending monitoring for applications and systems](#provide-analytical-and-trending-monitoring-for-applications-and-systems)
     - [Log / Audit everything](#log--audit-everything)
     - [Provide transparency into web operations systems and deployment methodologies](#provide-transparency-into-web-operations-systems-and-deployment-methodologies)
     - [Provide an open structure that enables us to better support the open web and the Mozilla community](#provide-an-open-structure-that-enables-us-to-better-support-the-open-web-and-the-mozilla-community)
     - [Provide a better customer experience](#provide-a-better-customer-experience)

## On listening to our customers
We spend a great deal of time designing systems. We are really good at that. Just about any one of us can be handed a technical problem to solve, go off and figure out how to solve it, then implement our solution. We totally have that covered. Where things start to get tricky for us is when we need to work with other people and collaborate on the solutions they are working on. We are not great at coordinating within IT. Now, I think we, as an organization, understand that and we are working on it. The new capability model and city map are the start of what, I hope, will be a more cohesive way of working within our organization. I must say however, that we are not very good, at all, when it comes to understanding our customers needs. We do not encourage feedback from our customers, as a mater of course. We do not engage our customers when selecting problems to solve, let alone when choosing technology or defining processes. If we intend to remain relevant, we must figure out how to include our customers at all levels. We must start genuinely listening to them and taking their feedback to heart. We have a tradition in IT that says "We know best. We will build you what you need", this attitude simply must change. While we may be experts at the technology we specialize in, when it comes to our customers needs, we do not know best. We do not begin to know best or, in many cases, even understand their needs, let alone their wants. We can do this. We can listen. We can fix our processes to include feedback from our users, and we must do this.

## Examples of current state
We need to travel with our customers to where they want to go. We currently provide either bare bones VMs or complicated and convoluted web clusters for our infrastructure offerings. Lets look at each of those in turn.

Think about the bare bones VM offering. In some cases this can make sense, however we must consider that in order for a developer to use this system they need to have quite a bit of knowledge about linux operating systems. Granted many of them do, however in order to operate an application in a robust, secure and reliable manner requires a whole other level of knowledge and expertise. It is not reasonable for us to assume that our customers have that knowledge any more that it would be for them to assume we were experts in the technology they are deploying. I ask you, how many people here know how to debug the java compiler? How about go? Python? PHP? I could go on, but the point is that no one person can be an expert in everything. This is precisely why we specialize. We, as IT, specialize in delivering safe, reliable and secure systems. Yet we often over burden our customers with exactly what we are experts in.

Lets look now at the web cluster model. The developers have zero access into these systems. That is by design as they are shared systems and many of the applications running there contain sensitive data. The customers do not have any insight into the deployment of their application, they can not even see the settings file for their application. When it comes to troubleshooting, we do not have a way for most of our customers to even get at their application logs. If there is ever an issue, they must contact us, then we attempt to troubleshoot their application, built on a technology that they specialize in and, that by definition, we do not. When we think about the experience behind that, how our customer must feel, frustrated, under-served, like IT has a feeling of superiority and a control they will not relinquish. I am not making this stuff up, this comes directly from numerous conversations that I have had with our customers over the years.

In both of the examples above we have a situation in which the subject matter expert is operating outside of their field of expertise. On the one hand a developer is tasked with the low level tasks that a systems administrator specializes in. On the other hand we have systems administrators attempting to troubleshoot technologies that developers specialize in. Note well that I am not advocating against a devops model. In fact I am advocating for a devops model. One in which we, as IT, work closely with the developers to create a devops environment.

## How can we Improve
So, how do we fix this? How do we provide our customers with what they need? First, we must engage our customers, second we need to provide them what they need and desire. Our customers are asking for things like; faster turn around time for systems, more self-service opportunities, more control over their own applications, more insight into the systems running their applications, options for running in containers, support for more third-party (SAAS & PAAS) offerings, a better password experience, better ticketing systems, and the list goes on. I know that many of these issues are being addressed in and around IT. For our part here today we are going to be discussing our infrastructure offering, our application deployment offering and self service opportunities. We are going to start by taking a look at how we operate in the datacenter and look for some areas where we can discover opportunity for improvement.

## Current operating model
If we look at the way we operate in the datacenter we discover that a number of our current practices are based on certain constraints. For example, it takes quite a lot of time and planning to upgrade to a bigger, more robust, server. From specking to purchasing approval to shipping time, just to get the hardware on-site. Then the datacenter technicians need to rack and cable the hardware as well as install a basic operating system. In my experience this process can take anywhere form about six weeks to more than a year, for larger projects. Lets look at another example, say I need to increase the disk space in one of my servers. I still need to go through the specking, purchasing and shipping process to get the new drives on-site. However once they are there a process starts where I coordinate with the datacenter technicians to swap out one disk at a time while I monitor the RAID rebuild status. I am sure we have all experienced issues rebuilding RAID arrays that range from hours upon hours of degraded performance, while corrupt arrays error check and rebuild, to complete loss of array integrity and data. Time for the backups, we have good backups right? This assumes that I have RAID on-board and don't need to do an application migration to accomplish a disk upgrade.


### Illustration of current challenges
Late me take a moment to talk about people making manual changes. When I was working on a web operations team, a developer for one of the web sites on one of our web clusters contacted me about an intermittent issue with their application. When loading some of the pages, there would occasionally be an error rendering portions of the page, however a reload would often fix the issue. I began troubleshooting and after quite a bit of time noticed that the errors only happened when one particular web server was serving up the page. Having narrowed the issue down to a single server I focused in and began trying to determine why that one server was misbehaving. After several days I was getting desperate (AKA my bag of tricks was nearly empty). I decided to just start comparing installed package version with other web servers. I discovered that a library on the misbehaving web server was at a slightly newer version than on the remaining servers in the cluster. Not knowing why this library was upgraded or how it was upgraded, I went to the developer to ask if they thought this could be the issue. The developer told me that in fact there was a small change in the library that would break their application. Armed with that knowledge, I began trying to figure out what, or who, might have upgraded this library. You see I was concerned that if I simply downgraded the library that I might break something else. I started with the usual suspects, was there a change in puppet that had not made it to the remaining servers? Did someone not pin a version and perhaps this one server came on-line at a later date than the other servers, hence getting a newer version. Perhaps another one of my team members upgraded it when troubleshooting a different issue? After exhausting my set of inquires as to the origin of the upgrade, I decided to just take the risk and downgrade the library version to match the other web servers in the cluster. I mean, after all the site was serving up errors, and had been doing so for at least three days by this point. I executed the command using the package manager and, as package managers do, it listed the other changes that it would need to make to the system to satisfy my request. In the list of changes was removal of a command line tool that I had installed a week prior in order to troubleshoot a separate issue. It turned out that I had been the source of the trouble all along.

There are several problems illustrated with that example. There was no way to ensure that all of the servers in the cluster were identical. There was no way to tell who or what might have made changes to the system. Due to the fact that we were hosting multiple applications on a single cluster, there was no way to know if my downgrading the library would not break another site. There were to many ways a system could be modified. There was a lack of monitoring resulting in the developer reporting the issue, this is not a good customer experience. The human process around troubleshooting did not include ensuring the environment was pristine on logout. There were no logs describing sudo user commands, which could have been used for historical information, I simply had to ask around and rely on peoples memory. Critical packages were not pinned at specific versions. The Puppetmaster methodology attempts to ensure eventual consistency but can only be deterministic about assets it is aware of. There are probably more things we can point out that are less than ideal in this example, but I hope you can agree that there must be a better way of operating.

### List of issues in our current work-flows:
Lets take a moment to dig into some of the challenges we currently face.

 - [Tainted resources](#tainted-resources)
 - [Lack of package pinning](#lack-of-package-pinning)
 - [Untested changes to production systems](#untested-changes-to-production-systems)
 - [Lack of isolation between applications](#lack-of-isolation-between-applications)
 - [Too many ways a system can be mutated](#too-many-ways-a-system-can-be-mutated)
 - [Puppetmasters ensure eventual consistency](#puppetmasters-ensure-eventual-consistency)
 - [Puppet's inability to guarantee symmetry among systems](#puppets-inability-to-guarantee-symmetry-among-systems)
 - [A word on backups](#a-word-on-backups)

#### Tainted resources
The idea of tainted resources will be quite new to those of you who have spent most of your careers lovingly hand crafting artisan systems. That is to say, for a long time we in IT have set up systems by hand, sure we sprinkle in a bit of bootstrapping automation and throw a little puppet on top, but we still find ourselves tweaking things by hand in order to get optimal performance. By definition these servers are tainted even before they get put into production, meaning there are customizations on the servers that are configured by hand, and not by automation.

Tainted resources are bad because they are not repeatable. There is no way to guarantee one system will be identical to another. This may be a desirable trait in art, however it is a potentially disastrous trait in computing. We need to know with as near to 100 percent accuracy as possible that systems are identical. For example, we need to know that the production environment, which we are getting ready to deploy a new version of an application into, is identical to the staging environment we have just completed successful testing in.

This brings us to tainted resources. We must consider any system that has been modified from its well defined state to be tainted. This means that any local modification taints the resource. Further any tainted resource should be destroyed and rebuilt from a golden image. As you can see, basically every asset we run in the datacenter is by definition tainted and, if we are interested in reliability, should be destroyed and rebuilt from a golden image. We will talk more about golden images and the exact definition of tainted resources in a little bit.

#### Lack of package pinning
This problem seems innocuous but is actually quite insidious. Without pinning all packages to a particular version it is not possible to know if an application will run without errors. If a developer is working with one set of package versions and the web servers are running a different set of package versions, it will eventually lead to errors. This goes one step further when working with web clusters. As seen in the example above, it is possible to have different versions installed on different servers within the same cluster. It therefore becomes quite important, in terms of reliability to have some way to ensure that we are working with the same set of dependencies across every environment that an application will be deployed into.

#### Untested changes to production systems
While we have some dev and staging environments, these only cover a narrow set of circumstances. For example, we have a staging server to test a web application before promoting it to production. We do not however, have a staging system for puppet. This means that any changes made to puppet code are tested in the production environment. In fact, I have taken down the majority of our Apache servers whilst making a change to out base Apache Puppet module. Further more, we do not have a testing environment for our monitoring system, for our code deployment pipeline, and many of our underlying systems (DNS, DHCP, NTP, etc) do not have staging environments at all. I do not think I need to go into the details of why this is an issue, but suffice it to say this has caused many outages that could have been avoided with a way to test changes before they went into production.

#### Lack of isolation between applications
This is a multi-part problem, however the basic issue is simple. First, due to the use of web clusters, it is not currently possible to know which dependencies were installed for which application. This leads to a situation where upgrades are difficult as it is not possible to know with any certainty that an update for one application will not break another application. Second, most application servers are configured from a single puppet module with little separation of dependencies. This leads to the same issue but on a wider scale, in other words it is nearly impossible to ascertain if upgrading a dependency for one application server will affect another application server. Both of these causes attain the same result, operators are hesitant to upgrade any dependencies due to fear of causing more issues. This leads to a situation where systems are quite out of date and open to security vulnerabilities. Further it creates a situation where our customers are often required to re-code their applications in order to use the old dependencies we have installed on production systems.

#### Too many ways a system can be mutated
There are many ways a system can be changed today. Puppet controls some of the system. Administrators can log on and make manual changes. Automated deployments of application code which are, in many cases, not tracked or logged. Developers can manually deploy changes including changing packages through language specific package managers like pip or npm. During security fire-drills administrators make changes in a wily-nilly fashion, often with little regard to testing. External security auditing tools, like Mig, can modify the state of a running system. The list goes on, but hopefully you understand the issue. With so many ways a system can be modified it creates a situation which makes troubleshooting difficult. In fact it is often difficult to know with any certainty that an application will run at all, let alone without errors.

#### Puppetmasters ensure eventual consistency
Puppet in the datacenter attempts to ensure eventual consistency of assets it is aware of. This leads to a number of issues. When upgrading or adding a new package to a series of servers, there is a time when different systems have different versions. This is not handled in a controlled rollout, but in an ad-hock manner which often leads to inconsistencies during this period. Puppet does not control the entire system, hence there can be a lot of inconsistency between servers. When we look at updating a package through puppet in the datacenter the work-flow is; user updates file on locally checked out copy, user pushes changes to version control system, puppetmasters eventually update from a cron job, taking upwards of 15 minutes, individual servers update on a 30 minute schedule. This means that without sudo user intervention, the process, from the time the user checks in code to the time all servers are updated, can take over an hour. There are very few standards around Puppet, leading to messy and inconsistent puppet modules as well as issues with puppet version upgrades as well as interoperability issues between various linux distributions. Version pinning is optional, this leads to version mismatches between servers in clusters or environments.

#### Puppet's inability to guarantee symmetry among systems
This point can be illustrated by asking a simple question "Can you guarantee that two servers in your web cluster are identical?". The answer is "No". This is no fault of puppet, but rather that puppet does not control, or even know about, all of the packages and configurations installed on a given system. Puppet can only control the assets it is aware of, and that is typically a fraction of the packages and configurations that are actually on a system.

#### A word on backups
When was the last time you verified your backups. I do not mean that they are running on the correct schedule, I assume you have an alert for that. I am talking about actually using a backup to restore your system to verify it actually works. I posit that without this test there is no way to know that a system can actually be recovered. I would further suggest that backups should be restored routinely and automatically. This brings me to my point. Any process that is not tested routinely can not be expected to work when it is needed. Any process that is not automated will eventually fail due to errors. There are several places where errors creep up during manual processes; the first and perhaps most obvious is simple human error (colloquially known as fat fingering a command), the second and perhaps most insidious error happens when something on the system is different from when the process was created or last tested. There are many many ways for things to change in the way we operate in the datacenter, as we have already discussed.

## User experiences
TODO: List some user experiences that exist today that we can improve upon. The idea here is to humanize the issues we are addressing. To reinforce the ideas presented above. (Might not be necessary???)
 - Time to market for new account
 - Time to enact small changes
 - lack of transparency
 - etc...

## Future Operating Model
At this point we should all have a general understanding of some areas in our current work-flows and processes that could stand some improvement. Quite some time ago I sat down with a few of my coworkers and we began to brainstorm around how we could improve on our current situation. In doing so we came up with some basic concepts and principles under which we should operate.

We have all of these ideas of these specific technological pieces that we want to improve upon, but it is not enough to simply say "make better" or improve upon. We instead need to be able to define what it means to make better or improve upon. We started by taking this list of things that did not work as well as we would like and tried to identify a work-flow or ideal system for each of these. It became clear from the start that we were talking about creating a system that was highly agile and automated. It was also clear that we had to get out of the way of the process. We needed to remove ourselves from the path of work as much as possible, in other words get rid of the "human API". In order to achieve this we have a need to provide, not only a high level of automation, but also a high level of self service. In other words, we need to empower our users to be able to deploy their applications and systems without going through us. This is a shift in paradigm, a new way of thinking for us. This creates an opportunity for us to reevaluate our roll in delivering systems to our end users. It also points out the need to think about standards and security right from the start. There is a little process built in to this system, a sort of bargain or agreement that we all have to buy into or accept, but as much as possible we want to stay out of the process. We want to simply deliver an awesome platform that the end users can develop their own processes around. We need to provide them the tools so that they can build the process they require, without muddying the waters by adding our own process. Everything must, therefore be well documented, in as open a place as possible. This leads us to some basic tenants of this new system:

 - Everything must be well documented. Preferably, things should be self documenting.
 - We should use open source, community built and maintained pieces wherever possible.
 - Each piece we build should be designed to be reusable as well as open source.
 - Everything should be developed in the open and be available for review, comment, pull-request, etc.

It is important to note that we are not trying to provide a datacenter automation strategy. We are operating in a new arena, specifically the cloud. Our management team has decided that we must limit ourselves to working solely with the Amazon cloud offering, Amazon Web Services (AWS).

Put succinctly we are trying to provide a framework for deploying applications in a simple, self-service, automated, and repeatable way. Note here, we consider everything that runs on a system to be an application regardless of its complexity or protocol. In other words, a simple web site is an application. Additionally a DNS server is an application, as is a monitoring system or a metrics system. We make no distinction for what is deployed, they are all treated equal.

This brings us to some high level ideals about what we are trying to accomplish. These include:
 - Delivering standards that users can design applications around
 - Providing self service opportunities to our customers
 - Providing automated; provisioning, networking, firewalling, monitoring, backups, load balancing, autoscaling, etc
 - Ensure basic security through; application isolation, ssl certificate automation, code review, InfoSec review (RRA), etc...
 - Provide a comprehensive, analytical, trending monitoring suite covering the entire application stack

### Specific areas of improvement
Lets take a few minutes to dig into some details of some things we can do to help alleviate some of our current headaches while providing some of the benefits we were just discussing.

 - [Automate all the things](#automate-all-the-things)
 - [Built on cloud technology](#built-on-cloud-technology)
 - [Provide self service opportunities](#provide-self-service-opportunities)
 - [Create standards](#create-standards)
 - [Treat datacenters as reusable components](#treat-datacenters-as-reusable-components)
 - [Exterminate the "Human API"](#exterminate-the-human-api)
 - [Use more community resources](#use-more-community-resources)
 - [Revision everything](#revision-everything)
 - [Transition work-flow to GitHub](#transition-work-flow-to-github)
 - [Code Reviews](#code-reviews)
 - [Provide Application isolation](#provide-application-isolation)
 - [Provide a platform that can autoscale](#provide-a-platform-that-can-autoscale)
 - [Bit for bit repeatable deployments](#bit-for-bit-repeatable-deployments)
 - [Destroy Tainted resources](#destroy-tainted-resources)
 - [Reduce time required to stand up a new application](#reduce-time-required-to-stand-up-a-new-application)
 - [Provide analytical and trending monitoring for applications and systems](#provide-analytical-and-trending-monitoring-for-applications-and-systems)
 - [Log / Audit everything](#log--audit-everything)
 - [Provide transparency into web operations systems and deployment methodologies](#provide-transparency-into-web-operations-systems-and-deployment-methodologies)
 - [Provide an open structure that enables us to better support the open web and the Mozilla community](#provide-an-open-structure-that-enables-us-to-better-support-the-open-web-and-the-mozilla-community)
 - [Provide a better customer experience](#provide-a-better-customer-experience)

#### Automate all the things
This is an important first step on our journey. We need to automate as many thing as possible. This removes many of the issues related to human error or "fat fingering" commands. This also has the potential to greatly reduce time to market for many of the services we offer. It is also a key milestone on the path towards self service. When we automate things we open ourselves up top the opportunity to put our substantial experience down in code so that our customers can take advantage of it time and again.

A few of the things that we could automate include:
 - Provisioning
 - Networking
 - Firewalling
 - Monitoring
 - Backups
 - Load Balancing
 - Autoscaling
 - SSL Certificates
 - Proxies
 - Log Aggregation
 - Systems DNS
 - User management
 - SSH Keys
 - Jumphosts
 - IP Allocation
 - DHCP
 - VPNs
 - MTAs
 - Databases
 - Memcache

The list goes on and on...


#### Built on cloud technology
The idea behind building on top of existing cloud technology is that it gets us out of the datacenter game. We are historically not very good at predicting our datacenter footprint which has led to some challenges. We find ourselves with more than double the necessary footprint. This is compounded by the desire to provide some level of high availability. Additionally we get locked into multi-year contracts with no room to adapt when our needs change. More than that however, it the idea that we can get at compute and other underlying services more rapidly. While we have been using more and more VMs in the datacenter, the cloud offers a number of additional services such as; ready made databases, virtual networking, zero capacity planning requirements, and so on.

#### Provide self service opportunities
In my opinion the primary reason that we are loosing our customers to other departments, PASS and SAAS providers, and so on, is that we are not providing what our customers need in the time they need it. While providing them what they need may be a larger endeavor, providing them services in a timely manner is completely withing our grasp. It starts with automation as stated previously, but that flows directly through to self service. This is the ultimate convenience that our customers are gaining elsewhere. With few exceptions, our customers are more interested in developing and running their applications than they are in operating systems. If we can provide them with completely configured, reliable and secure systems at the push of a button that they do not have to maintain, I believe this will be quite attractive to them.

#### Create standards
This one is huge. We have a tradition of "cowboy ops". This method of working served us well when we were a small organization, poorly staffed and more focused on getting stuff done than long term viability and maintainability. However, this has led to a situation where nearly every system is its own "snowflake". Beautifully crafted and only maintainable by the artist that created it. While this might make for a lovely museum instillation, it does not serve us well in todays increasingly agile world.

In order to remain relevant we need to embrace agile. We need to become more agile as well. The only way we are going to be able to do that is if we accept the fact that we need to start doing things in a standardized and repeatable way. In a way that allows any technician to be able to troubleshoot any part of the system. We need to put ourselves in a position where we are not always required to be "rock stars" who undertake herculean efforts at every turn. We should not be striving to be despot cowboys, independent and lonely. We need to come together and form a team. Working together as a team we will be able to accomplish far more than we could ever dream of when working alone. Creating a framework, or standards, that we can agree on is one of the first steps towards that goal.

#### Treat datacenters as reusable components
This is a bit of a misnomer. We are actually moving away from the datacenter model. I am using the word datacenter here to denote the set of core services that every application needs regardless of being a physical datacenter or a deployment on some cloud technology. This core set of services contains things like; DHCP, DNS, NTP, proxies, firewalls and so on. The point here is that this core set of services should be configured in a way that we can reuse it over and over without spending the least bit of time on configuration. These services are for the most part so simple and so well understood that little time should be spent on them. We have more interesting and more important things to spend our time on that configuring a DHCP server for the millionth time.

#### Exterminate the "Human API"
Today, a customer files a bug in Bugzilla or ServiceNow and then waits around for some person to pick up the request. Often times the request requires little more than firing off some scripts or making some small change to Puppet. The issue arises due to the fact that we are all quite busy and therefore requests often sit for days or weeks before any action is taken. Then when action is taken it is often in the form or asking for additional information. This back and forth along with shifting priorities often leads to substantial delays in fulfilling even the simplest of requests. This Bugzilla (ServiceNow) driven back and forth is what we lovingly refer to as the "Human API".

We need to get rid of the "Human API" for each and every request, where it is feasible to do so. This is made possible through both automation and self service opportunities. This serves us in a number of ways. First it frees us up to spend more time on the things that actually matter. Second it decreases the turn around time for our customers simple requests, which in turn increases their happiness (delight).

#### Use more community resources
This is truly a force multiplier. Sure, we can create everything from scratch, we are crazy capable. The question is not one of ability but of time. I for one would rather be working on the complex and interesting problems of today instead of consistently reinventing the wheel. Practically speaking we can not create everything from scratch. The real question is where do we draw the line. It is my measured opinion that we should use open source technology at every opportunity. If there is a minor thing lacking, we should add it and contribute it back to the open source community.

We have a habit here in IT of using community technologies whilst giving little back. I think it is time that changed as well. I will mention that, for my part, it is far more gratifying to contribute one small patch to an existing open source project than to reinvent ten projects on my own. There is something exciting and rewarding about knowing that your contribution will serve more than just your needs.

As an example, we are taking advantage of [PuppetForge](https://forge.puppet.com) for community puppet modules. This was not a resource that existed years ago when we adopted puppet in the datacenter. This exemplifies the difficulty in changing direction and the cost of technical debt.

There does come a point when no tool exists that can do what we need to accomplish. In such cases we can create our own open source project to fit the need. This requires a lot of careful consideration as there is a lot involved in running a successful open source project. From releases to keeping things up to date to managing community contributions, it is a lot of work and requires an investment in time. It is almost always better to find a project that almost fits and submit patches.

#### Revision everything
If every part of the system is revisioned in some way then it will always be possible to recover to a known good state. That is the basis for using a Version Control System (VCS) for deploying assets in the cloud. Now, there are a number of additional benefits to using a VCS, but this point can not be overstated. It does not matter what changes are made to a system, so long as you know when the last working state was you can always revert.

#### Transition work-flow to GitHub
GitHub provides a number of advantages over our current Subversion based work-flow. GitHub takes git as a VCS a step further. It makes it trivially simple to created patches and collaborate on them. The forking and branching methodologies that are inherent in git provide an opportunity to test code in a safe way that does not risk changes to production systems. Adding on the open collaborative enhancements that GitHub provides, gives us an easy to use, out of the box, experience that aligns quite well with agile methodologies.

#### Code Reviews
Utilizing the work-flow that git and GitHub provide we find ourselves in a place where code reviews become, not only possible, but quite simple. While we transition away from "cowboy opps" into a more mature working model we find ourselves needing to reduce the number of human errors, "fat fingering", wherever practical. Code reviews are one way in which we can gain a lot of benefit for little effort. Code reviews are common in most development shops today and, if it is not yet obvious, when working with cloud technologies, we are working entirely with code. This transition brings with it a number of opportunities to improve. Code reviews are an excellent way to not only reduce human error but also to engender collaboration and teamwork.

I have found great advantage in having my code reviewed. I have learned a number of new things, prevented a number of errors from reaching production, as well as developed closer relationships with my coworkers in the process.

#### Provide Application isolation
Working with cloud technologies provides us with another great opportunity. Namely we have the opportunity to deploy applications in their own little "datacenter" islands. This separation, or isolation, gives us a number of exciting advantages.

First, applications are isolated from each other in terms of resource consumption. This means that if one application suddenly has an increase in traffic and begins consuming a large number of resources, it will not adversely affect any other applications. This is true not only on the compute layer, but also on the database layer, the caching layers, the load balancing layers, and so forth.

Second, application isolation gives us the ability to express explicitly what dependencies a particular application has. This enables us to describe precise versions which are specific to an application. It further provides us a safe and discrete way to upgrade dependency versions with out fear or risk to any other application.

Application isolation in the cloud also provides us the ability to understand the cost associated with operating any given application. While this is less an operator need, management sure finds the information handy. Truly it is a very useful tool for the company in evaluating weather or not a particular application is worth operating at its current level or if it needs to be reevaluated.

#### Provide a platform that can autoscale
Autoscaling is a new concept when operating on cloud based technology. In AWS, the cloud we are currently deploying on, they offer basic vertical scaling. Given this ability it becomes possible to allow applications to right size themselves for their current load. This has multiple advantages.

When an application suddenly comes under increased load, the underlying compute layer can scale up to accommodate the demand. This can occur regardless of where the increased load is coming from. It can be from an engagement campaign, or seemingly just as likely, form a developer pushing inefficient code.

As soon as the event requiring increased resources has passed, the application compute layer can scale back down (in) which effectively reduces cost. This means that we no longer need to size resources for the "worst case" scenario. Rather we can size for normal load and rely on autoscaling to kick in when demand requires.

This is a form of automatic right sizing. When configured smartly, autoscaling can provide great benefits to both cost and reliability.

#### Bit for bit repeatable deployments
It is time now for us to dream the impossible dream. Imagine, if you will, a world in which we could deploy an application to production with 100 percent certainty that it would work. Not just start, but actually work as designed, right down to the smallest detail. I say to you, this dream can be reality. It starts with the concept of, Golden Images

A golden image is simply a compute (VM, EC2, etc) resource image that does not change when it is put into operation. In other words, no packages are upgraded, no scripts run which mutate state, code is not updated, and so forth. By approaching images in this way we can guarantee that if we launch an instance using this image today, that tomorrow we can launch the same instance using the same image with the exact same results.

Now if we take this "golden image" concept up a level, to include the entire deployment, we get to the idea of bit for bit repeatability. Meaning that every resource launched, from the databases to the web servers to the load balancers, are all based on golden images.

By deploying the entire environment this way we can have, say a staging environment in which we run tests to validate functionality. Then when we deploy the exact same, bit for bit, environment to production, we get to do so with a certainty that we have never been able to achieve before.

While this presents some interesting challenges in application, it none-the-less provides us with a degree of confidence that is unprecedented. It enables us to contemplate approaching things in new ways. For example, we could consider destructive, failure based, testing in a staging environment. We can deploy test branches, try out new concepts, all the while confident that if it works in our test environment it will work, with certainty, in production.

This may very well mark the beginning of a new era of enlightenment in IT... What, am I overselling it a bit? Well okay, perhaps a bit, but still you have to admit, this is really cool.

#### Destroy Tainted resources
This brings us right along to the concept of tainted images. A tainted image is any image that has differed from its golden image. While that sounds simple enough, I understand there is a bit of confusion around what exactly constitutes a tainted image. In other words, what exactly makes an image tainted. Simply stated, any configuration or system level change that would persist across a reboot taints a system.

I think it is simpler to explain what changes do not constitute tainting an image. Things like running state, meaning changes to running memory (ram). Likewise changes to the '/tmp' file-system. Mounting a network attached storage volume. Any thing that is a transient state that does not persist over a reboot can generally be considered non-tainting.

Conversely, anything that would persist over a reboot necessarily taints the resource. For example, upgrading a package would taint the resource. Locally modifying a configuration file or updating application code modifies the system in a persistent way and therefore taints the resource.

You may be asking yourself, "If I can't modify a configuration file, how do I configure my application?". Excellent question. I am glad you asked. Allow me to answer that by way of an example. Take database configuration. We understand that we need a username and password to connect to our database. Lets assume that we do not know this when we create our golden image. We simply make those pieces of configuration inputs that we pass to the golden image on boot. This way the configuration file, with variables in place, does not change on boot and the instance simply exposes the variables to the application on startup. We will see specific examples a little later on.

The key takeaway when considering tainted resources is this. You can never trust a tainted resource to be 100 percent identical to a known good image. Therefore it is wise to terminate any suspected resource and allow autoscaling to replace it with a fresh, known good, golden image.

#### Reduce time required to stand up a new application
When we start to consider these concepts as a whole, we start to see how we could add them together to advantage. When we take automation together with golden images, we begin to get to a point where speed of deployments along with the level of confidence we can achieve, combine to give us something special. We can start to see that we can reduce time-to-market by a wide margin. When we sprinkle in a bit of self service, our turn around time starts to diminish as well.

By taking all of that together with some agreed upon standards of practice, we begin to arrive at a place where we can stop spending cycles configuring the same old Apache server and start fine tuning automation to deliver instead. When we start to automate ourselves out of the mundane tasks, we not only free up our time to focus on more interesting challenges, we also provide a better experience to our customers.

#### Provide analytical and trending monitoring for applications and systems
As we start to become more hands off with basic deployments, we discover a need to really monitor the things that matter. While monitoring our artisan servers, we would look for things like load going beyond some number. We would monitor things like XX gigs of free disk space. This worked well because we were basing our monitoring on hardware that we specked, purchased and installed. Hardware that did not change often.

It the cloud we can no longer monitor in this manner. The underlying resources, hardware if you will, changes so rapidly that our monitors become out of date almost as soon as we have them in place. Instead of monitoring the quantity of disk space, we need to monitor for the amount of time until a disk fills up. Instead of monitoring load on a single, or cluster of servers, we need to monitor unexpected changes in the resources required to accomplish a given task.

Further, it is not sufficient to understand if the underlying resources are adequate and performing up to requirements. It does us absolutely no good to know that we have a perfectly happy and stable web server if all it is serving up are 404s. We need to get to the point of functional monitoring.

Functional monitoring is not simply understanding the health of the underlying systems, although that is a part of it, but rather understanding the health of the entire application. We need to start to understand the performance of the application itself. For example, search latency trends over time, or responsiveness over time. We need to understand if form submissions are actually making it to the database. In essence, we need to instrument the application based on its intended function and then understand if the underlying resources are adequate to the task.

Given that we now have application isolation and bit for bit repeatability, we are able to open the door to understanding the applications we are running. We can no longer be satisfied by simply stating "The server is up.". We must strive to be able to say "The application is operating flawlessly!".

#### Log / Audit everything
We collect a lot of logs. Most of them never leave the host system on which they were generated. The few systems that are set up to aggregate logs, generally only process a fraction of the logs they receive. Then the ones that are processed are only available to a few people and getting at any meaningful insights is difficult at best.

The shame of this is that it is not difficult to make the situation better. There are a host of open source tools available for aggregating, parsing, map reducing, and making search-able logs of all sorts. We simply need to take advantage of what is already available to us.

Simply aggregating logs and making them available for parsing, opens up a world of insights. These insights not only help us to troubleshoot issues, but help us to predict issues before they become issues. Further this could be an invaluable resource to our customers when it comes to tracking down issues with their applications or simply understanding why something is operating in the way that it is.

Understanding our logs is more critical in the cloud. Resources are transient and often autoscale out of existence before we know there is an issue. It is commonplace for numerous scaling events to take place during the time it takes to investigate an issue. Therefore it is more critical in the cloud than in a datacenter to aggregate logs in a persistent manner. It is obvious that logs need to be retained in a persistent manner, but what is less obvious it the need to be able to get at the data within the logs in a rapid way that lends itself to insights. It is far to cumbersome to download and grep through gigs upon gigs of log entries, this simply does not scale. We need to have a system in place that makes this sort of investigation easy and fast.

#### Provide transparency into web operations systems and deployment methodologies
One of the more difficult issues facing us in the datacenter is balancing the need for security with the need to access systems. We have generally erred on the side of security and completely lock our customers out. This creates a situation where they have no insight into the systems running their applications. This in turn leads to the situation where the customer is developing blind. As a result they ofter build applications that do not run, or are plagued with errors, on the systems we provide.

This demonstrates the need for increased transparency at a minimum. Beyond that, I suggest that we have an opportunity to collaborate with our customers, early in the process, to provide them with systems that meet their needs. At the same time we can achieve a high level of security and reliability. It is a challenge to be sure, but one we can solve.

By placing all of the cloud configuration code in git, we are providing the customer insights into their deployment. We are also offering them the ability to collaborate on their deployment. This in conjunction with using open source technologies grants our customers hitherto unknown insights.

In addition to the above we should document everything possible in an open and collaborative environment. We should not place any documentation in a close walled garden. Everything from design documentation to HOWTOs should be public. This, of course, necessitates care so as to not expose any secrets. We should continuously strive to be more open and more transparent.

#### Provide an open structure that enables us to better support the open web and the Mozilla community
By now, we have some ideas based on open source technologies, documented in a public way. We can enable bit for bit repeatable deployments with security and reliability at the forefront of the design. We have created an environment in which we can work in the open while collaborating with our customers.

This will enable us, practically for the first time, to be able to contribute to the open source community as an organization. This is more than an open source first stance, this is an open source collaborative stance. I truly believe that when we all pull in the same direction and work together, we can be more successful than when we work as lone cowboys.

This not only serves our own ends, but also our customers and finally helps to foster a more cohesive Mozilla community.

#### Provide a better customer experience
All of this adds up to a better customer experience. As I said in the opening, we are facing some unprecedented challenges in IT today. I firmly believe that if we accept that the way we have been operating is in some ways flawed and that there is room for improvement, we can truly make things better.

At the end of the day we work in a services industry. We must serve the needs of our customers first. We must move away from the "IT knows best" mentality and into one of collaboration and inclusiveness. In this way and by adopting the types of practices I have outlined here, we can get better. We can win back our customers, and I dare say, we can have fun doing it.
