# Index
Welcome to the Nubis training documentation. As of this writing this is very much a work in progress. The table of contents is currently designed to help me to design the course content and help me keep track of my progress and what I have left to accomplish. This list will eventually link out to the content covered by each section.

## Table of Contents
This section contains the material we will cover in the classroom session. Eventually we hope to develop this into a self-guided training course that a person can work through at their own pace.

0. [Introduction](./introduction.md)
 - [On listening to our customers](./introduction.md#on-listening-to-our-customers)
 - [Examples of current state](./introduction.md#examples-of-current-state)
 - [How can we Improve](./introduction.md#how-can-we-improve)
 - [Current operating model](./introduction.md#current-operating-model)
  -  [Illustration of current challenges](./introduction.md#illustration-of-current-challenges)
  - [List of issues in our current work-flows](./introduction.md#list-of-issues-in-our-current-work-flows)
     - [Tainted resources](./introduction.md#tainted-resources)
     - [Lack of package pinning](./introduction.md#lack-of-package-pinning)
     - [Untested changes to production systems](./introduction.md#untested-changes-to-production-systems)
     - [Lack of isolation between applications](./introduction.md#lack-of-isolation-between-applications)
     - [Too many ways a system can be mutated](./introduction.md#too-many-ways-a-system-can-be-mutated)
     - [Puppetmasters ensure eventual consistency](./introduction.md#puppetmasters-ensure-eventual-consistency)
     - [Puppet's inability to guarantee symmetry among systems](./introduction.md#puppets-inability-to-guarantee-symmetry-among-systems)
     - [A word on backups](./introduction.md#a-word-on-backups)
 - [User experiences](./introduction.md#user-experiences)
 - [Future Operating Model](./introduction.md#future-operating-model)
  - [Specific areas of improvement](./introduction.md#specific-areas-of-improvement)
     - [Automate all the things](./introduction.md#automate-all-the-things)
     - [Built on cloud technology](./introduction.md#built-on-cloud-technology)
     - [Provide self service opportunities](./introduction.md#provide-self-service-opportunities)
     - [Create standards](./introduction.md#create-standards)
     - [Treat datacenters as reusable components](./introduction.md#treat-datacenters-as-reusable-components)
     - [Exterminate the "Human API"](./introduction.md#exterminate-the-human-api)
     - [Use more community resources](./introduction.md#use-more-community-resources)
     - [Revision everything](./introduction.md#revision-everything)
     - [Transition work-flow to GitHub](./introduction.md#transition-work-flow-to-github)
     - [Code Reviews](./introduction.md#code-reviews)
     - [Provide Application isolation](./introduction.md#provide-application-isolation)
     - [Provide a platform that can autoscale](./introduction.md#provide-a-platform-that-can-autoscale)
     - [Bit for bit repeatable deployments](./introduction.md#bit-for-bit-repeatable-deployments)
     - [Destroy Tainted resources](./introduction.md#destroy-tainted-resources)
     - [Reduce time required to stand up a new application](./introduction.md#reduce-time-required-to-stand-up-a-new-application)
     - [Provide analytical and trending monitoring for applications and systems](./introduction.md#provide-analytical-and-trending-monitoring-for-applications-and-systems)
     - [Log / Audit everything](./introduction.md#log--audit-everything)
     - [Provide transparency into web operations systems and deployment methodologies](./introduction.md#provide-transparency-into-web-operations-systems-and-deployment-methodologies)
     - [Provide an open structure that enables us to better support the open web and the Mozilla community](./introduction.md#provide-an-open-structure-that-enables-us-to-better-support-the-open-web-and-the-mozilla-community)
     - [Provide a better customer experience](./introduction.md#provide-a-better-customer-experience)
0. [New operating principles](./operating-principles.md)
 - [New way of thinking](./operating-principles.md#new-way-of-thinking)
 - [Twelve Factor Review](./operating-principles.md#twelve-factor-review)
 - [Agile Development](./operating-principles.md#agile-development)
 - [Symantic versioning](./operating-principles.md#symantic-versioning)
 - [Code Reviews](./operating-principles.md#code-reviews)
 - [Decentralization](./operating-principles.md#decentralization)
 - [git and GitHub](./operating-principles.md#git-and-github)
 - [System level configuration](./operating-principles.md#system-level-configuration)
  - [Packer](./operating-principles.md#packer)
  - [Puppet (masterless)](./operating-principles.md#puppet-masterless)
  - [Consul and Confd](./operating-principles.md#consul-and-confd)
 - [Image Upgrades](./operating-principles.md#image-upgrades)
 - [To Autoscale or Not to Autoscale](./operating-principles.md#to-autoscale-or-not-to-autoscale)
 - [Tainted Instances](./operating-principles.md#tainted-instances)
 - [Security Requirements](./operating-principles.md#security-requirements)
0. [Exercise One](./exercise-one.md)
 - [Setup](./exercise-one.md#setup)
 - [Organize into groups](./exercise-one.md#organize-into_groups)
 - [Chose a Topic](./exercise-one.md#chose-a-topic)
 - [Discuss Improvements](./exercise-one.md#discuss-improvements)
 - [Presentations and Discussions](./exercise-one.md#presentations-and-discussions)
0. [Nubis overview](./nubis-overview.md)
 - [What is Nubis](./nubis-overview.md#what-is-nubis)
  - [Standardized design](./nubis-overview.md#standardized-design)
  - [Security compliance](./nubis-overview.md#security-compliance)
  - [Reduced time-to-market](./nubis-overview.md#reduced-time-to-market)
 - [What can Nubis do for me](./nubis-overview.md#what-can-nubis-do-for-me)
 - [What does Nubis provide](./nubis-overview.md#what-does-nubis-provide)
  - [Nubis accounts](./nubis-overview.md#nubis-accounts)
     - [Accounts](./nubis-overview.md#accounts)
         - [Account Diagram](#account-diagram)
         - [Multiple environments](./nubis-overview.md#multiple-environments)
      - [Quarterly Updates](./nubis-overview.md#quarterly-updates)
         - [Distribution upgrades](./nubis-overview.md#distribution-upgrades)
         - [Package updates](./nubis-overview.md#package-updates)
         - [New services](./nubis-overview.md#new-services)
         - [Application Image Updates](./nubis-overview.md#application-image-updates)
      - [Security Updates](./nubis-overview.md#security-updates)
      - [Included Services](./nubis-overview.md#included-services)
         - [Proxies](./nubis-overview.md#proxies)
         - [NATs](./nubis-overview.md#nats)
         - [Consul Integration](./nubis-overview.md#consul-integration)
         - [Fluent Integration](./nubis-overview.md#fluent-integration)
         - [Jumphosts](./nubis-overview.md#jumphosts)
      - [User Management](./nubis-overview.md#user-management)
         - [MFA](./nubis-overview.md#mfa)
         - [aws-vault](./nubis-overview.md#aws-vault)
         - [LDAP Integration](./nubis-overview.md#ldap-integration)
      - [Security Integration](./nubis-overview.md#security-integration)
         - [InfoSec security audit role](./nubis-overview.md#infoSec-security-audit-role)
         - [Network Security Monitoring](./nubis-overview.md#network-security-monitoring) (NSM)
         - [Integrated IP Blacklisting](./nubis-overview.md#integrated-ip-blacklisting)
         - [Log Integration with Mozilla Investigator](./nubis-overview.md#log-integration-with-mozilla-investigator) (MIG)
         - [CloudTrail Integration](./nubis-overview.md#cloudtrail-integration)
      - [Additional Services](./nubis-overview.md#additional-services)
         - [Cloud Health Integration](./nubis-overview.md#cloud-health-integration)
         - [Billing Support](./nubis-overview.md#billing-support)
         - [Tainted Resources](./nubis-overview.md#tainted-resources)
         - [Platform Monitoring](./nubis-overview.md#platform-monitoring)
         - [High Availability](./nubis-overview.md#high-availability)
  - [Nubis deployments](./nubis-overview.md#nubis-deployments)
      - [Deployment Overview](./nubis-overview.md#deployment-overview)
         - [Environments and how to use them](./nubis-overview.md#environments-and-how-to-use-them)
         - [Deployment Workflow Diagram](./nubis-overview.md#deployment-workflow-diagram)
      - [Deployment repository](./nubis-overview.md#deployment-repository)
         - [Puppet configuration](./nubis-overview.md#puppet-configuration)
         - [Application Code](./nubis-overview.md#application-Code)
      - [Terraform modules](./nubis-overview.md#terraform-modules)
      - [Recommended practices](./nubis-overview.md#recommended-practices)
      - [Architectural design services](./nubis-overview.md#architectural-design-services)
         - [Example deployments](./nubis-overview.md#example-deployments)
         - [nubis-skel](./nubis-overview.md#nubis-skel)
         - [AWS Solutions Architect](./nubis-overview.md#aws-solutions-architect)
      - [Community support](./nubis-overview.md#community-support)
      - [CI System](./nubis-overview.md#ci-system)
      - [Rolling Back](./nubis-overview.md#rolling-back)
      - [Custom Monitors](./nubis-overview.md#custom-monitors)
      - [nubis-base](./nubis-overview.md#nubis-base)
      - [nubis-builder](./nubis-overview.md#nubis-builder)
      - [Build Deploy Diagram](./nubis-overview.md#build-deploy-diagram)
0. [Exercise Two](./exercise-two.md)
 - [Chose a Topic](./exercise-two.md#chose-a-topic)
 - [Diagram the deployment](./exercise-two.md#diagram-the-deployment)
0. [Demonstrations](./demonstrations.md)
 - [Deploy a new application](./demonstrations.md#deploy-a-new-application)
 - [Deploy new application code](./demonstrations.md#deploy-new-application-code)
 - [Continuous Integration work-flow](./demonstrations.md#continuous-integration-work-flow)
 - [Upgrade an account](./demonstrations.md#upgrade-an-account)
 - [Troubleshooting](./demonstrations.md#troubleshooting)
0. [Working Labs](./working-labs.md)
 - [Setting up your local environment](./working-labs.md#setting-up-your-local-environment)
 - [Working with git & GitHub](./working-labs.md#working-with-git--github)
 - [Deploying the Nubis example application Dpaste](./working-labs.md#deploying-the-nubis-example-application-dpaste)
 - [Deploying your own application using nubis-skel](./working-labs.md#deploying-your-own-application-using-nubis-skel)
 - [Updating system level packages](./working-labs.md#updating-system-level-packages)

## Operational Documentation (HOWTOs)
Here are some links to context relevant HOWTOs which are intended to guide you through many of the tasks you will need to perform using Nubis.

 - How do I deploy an app
 - How do I login to AWS?
  - aws-vault overview (still might like a wrapper script for account setup)
 - Walk-through dpaste deploy
 - Build custom app with nubis-skel
 - Detailed working example for git and GitHub
 - How do I build an AMI?
  - Features of nubis-base
   - /etc/nubis.d/*
   - consul integration
  - Puppet masterless
   - Puppet modules
   - librarian-puppet
  - Packer overview
  - nubis-builder overview
   - distrobutions supported
   - project.json file requirements and options
 - How do I launch a jumphost?
 - How do I access instances
 - What is the meaning of immutable
 - What happens when my instance is marked as tainted?
 - How does monitoring work in AWS?
 - How do I upgrade my account to Nubis latest?
 - Terraform overview
 - Consul overview
 - Fluent overview
 - Proxy overview (including nat)
 - Database admin node
 - How do I add and remove users from my account
  - Levels of user permissions

## Technical Documents (Design docs)
In this section you will find links to some of our technical and design documentation. This material is intended to help you with troubleshooting. It is also helpfull if you would like to get into helping us with Nubis development.

 - NSM monitoring
 - IP Blocklist
 - Nat setup / HA / State
 - User Management
