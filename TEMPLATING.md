

# Templating

This document covers templating within the Nubis project. We have gone through a
few iterations of templating and are currently using Terraform for all
templating within the project. Further we recommend that any projects or
applications that deploy on top of a Nubis deployment also adopt Terraform as
their templating framework. While we place no restrictions on the deployment
framework or methodology that a team uses to deploy on top of the Nubis
platform, we encourage the use of Terraform for a number of reasons.

This document is divided into three main sections:

* [Recommended Practices for Terraform](#recommended-practices-for-terraform)
* [Terraform versus Cloudformation](#terraform-versus-cloudformation)
* [Recommended Practices for Cloudformation](#recommended-practices-for-cloudformation)
  (legacy documentation)

## Recommended Practices for Terraform

TODO

## Terraform versus Cloudformation

We have had a bit of back and forth with this decision. Technically speaking we
casually considered other deployment frameworks (boto ansible, etc...), however
we only put serious development time into Terraform and Cloudformation.

### A bit of history

When we started the Nubis project we decided to use Terraform in general. There
were folks on the team who did not agree and those folks used Cloudformation.
Some time into development we discovered a few limitations with Terraform,
specifically how it handled RDS, and reluctantly decided to use Cloudformation.
Eventually the limitations of Cloudformation led us down the path of extending
Cloudformation with Lambda functions.

We then got to a point where we were looking to streamline the account creation
and update process. As part of this streamlining we discovered that we needed to
use some sort of wrapper around Cloudformation in order to integrate with other
3rd party tools (DNS, IPAM, Monitoring, etc...). We did some requirements
gathering and began looking to see if there were any existing open source tools
or if we would need to create and maintain our own set of tools. It was at that
time that we decided to take another look at Terraform, it had been more than a
year since we had moved away from it and there had been a large amount of
development around it during that time.

We chose to do some prototyping of the account creation process using Terraform
along with both official and 3rd-party Terraform modules. We were really excited
with the progress that had been made and were delighted to discover just how
many of our needs were either already solved or in active development. We
decided, through much debate and deliberation, to go ahead and switch the entire
Nubis project over to Terraform and to discontinue our use of Cloudformation.

I should also mention for completeness that we managed to negotiate a limited
NDA with Amazon Web Services (AWS) and got a briefing on the Cloudformation
road-map. The information we got out of that meeting was quite informative and
we took that into consideration before making our final decision. While there
are some very interesting things on the horizon, we felt that Terraform still
offered enough additional features as to make it the obvious choice.

I was asked to document the rational behind the decision in the form of a
Terraform versus Cloudformation list. That list follows:

### Pros and Cons Matrix

| Feature | Terraform | Cloudformation |
|---------|-----------|----------------|
| Documentation Capability | Yes | No[^1] |
| Integration with 3rd-party tools | Yes | No |
| Amazon Native | No | Yes |
| Run-time Executions | Yes | No[^2] |
| Dry-Run | Yes | Yes |
| Access to Outputs | Yes | No[^3] |
| Multi-Component Dependencies | Yes | No[^4] |
| Cloud Agnostic[^5] | Yes | No |
| Dependency Graphing | Yes | No |
| Human Readable Configuration | Yes | No[^6] |
| Prescriptive Operation[^7] | Yes| No |
| Reusable Modularity[^8] | Yes | No |
| Open Source[^9] | Yes | No |
| Multi Region Deployments | Yes | No |
| Parallelized Resource Deployment[^10] | Yes | No |

[^1]: There is technically a metadata parameter hack that offers some limited
documentation, but IMHO this does not reach the minimum bar for any real metric
of documentation.

[^2]: Can handle *some* cases (ie: uuidgen) but it requires building and
maintaining lambda functions.

[^3]: We hacked this with a lambda function that required very precise
coordination of output maps to necessary inputs.

[^4]: We started writing a script that executed multiple cloudformation stacks
but ran into a lot of issues when things did not deploy perfectly.

[^5]: Our tool evaluation guidelines state that we should only consider AWS
specifically, however I included this for completeness.

[^6]: There are those who would argue that a machine interface language, having
been invented by humans, is human readable. I posit that while a computer
exchange format might be able to be understood by humans, and even authored by
them, that in of itself is not enough. When things, like ease of use,
readability, formatting, commenting, etc are taken into account JSON falls far
short of what I consider human readable or at least reasonable human manageable.

[^7]: During stack creation, if one or a few resources fail to create Terraform
has the ability to retry in a graceful manner. Cloudformation partial failure
handling does not exist and it is necessary to roll back the entire deployment.
During the development phase it is substantially faster when using Terraform due
to this behavior.

[^8]: Terraform has a large number of native and 3rd-party modules to do all
manner of things, like EC2 or RDS deployments. Cloudformation has no official
modular support. We did exploit the nested stacks towards this aim with general
success. It is worth noting that we never discovered any 3rd-party nested stacks
that were usable as-is.

[^9]: This is a really broad topic and I am consistently surprised at how little
credit is given to this point. There are numerous advantages to participating in
a healthy open source project over using closed source technology. I will
highlight a few here. With open source you can, well view the source code. This
is helpful for all manner of troubleshooting or, as we have done, to add
instrumentation to the tool to see where things have gone awry. With open source
projects you can, and we have, submit patches when you find issues. This ensures
that the issues that are of the greatest impact to you get prioritized right to
the top of the list. For issues that are not that urgent, you can subscribe to
the issues on GitHub and therefore get notifications when there is traction on
those issues, not to mention the ability to vote on issues and therefore
influence their prioritization. With any healthy open source project, like
Terraform, you can hang out in their public IRC channel and garner all sorts of
useful tips and tricks as well as the obvious ability to chat directly with the
developers, more or less, at any time. I could go on and on here but for now I
will digress the point.

[^10]: Terraform account deployments take around 5 minutes total. Cloudformation
deployments take around 40 minutes *per region*.

## Recommended Practices for Cloudformation

**NOTE: This section has been left for legacy reasons and is guaranteed to no**
**longer be accurate.**

Cloudformation is a necessary evil when working with AWS. It uses JSON which has
a number of staggering limitations. You will soon learn that it is overly rigid
in its formatting. Additionally it lacks commenting, which, as you know, is a
rather atrocious limitation. In an effort to limit your exposure to JSON we have
adopted a nested stack model. Basically you will create a stack template which
will use these ready made nested stacks. For an example check out this section
of the [nubis-mediawiki template](https://github.com/Nubisproject/nubis-mediawiki/blob/master/nubis/cloudformation/main.json#L70).

### Nested Stacks

Nested stacks are in and of themselves simply stacks that you include in a
higher level, or container stack. We have created a number of stack templates to
cover the most common use cases. You can take a look at them [here](https://github.com/Nubisproject/nubis-stacks).
For each stack template we have included a README which includes usage code that
you can copy into your stack template. Following the previous example from the
nubis-mediawiki project you can see the EC2Stack nested stack template [here](https://github.com/Nubisproject/nubis-stacks/blob/master/ec2.template).

### Stack Outputs

We have created a small [function](https://github.com/Nubisproject/nubis-stacks/blob/master/lambda/LookupStackOutputs/LookupStackOutputs.README.md)
that runs in [Lambda](http://aws.amazon.com/lambda/) (an AWS compute service)
which makes the outputs of other stacks available for reference in your
template. You will find us using this function in nearly every nested stack,
sometimes multiple times. While you may not find a need for this in your
template it is necessary knowledge for understanding the nested stack templates.
For example, in the EC2Stack example above we are calling the function as
[VpcInfo](https://github.com/Nubisproject/nubis-stacks/blob/master/ec2.template#L48)
and using the VpcId output of the $region-$environment-vpc stack [here](https://github.com/Nubisproject/nubis-stacks/blob/master/ec2.template#L73).

### Parameterization

By utilizing stack outputs we are able to minimize the number of parameters
(AWS name for input variables) we need. This simplifies deployments, especially
when multiple developers are working on the same project. Back in the
nubis-mediawiki project you will find the [parameters.json-dist file](https://github.com/Nubisproject/nubis-mediawiki/blob/master/nubis/cloudformation/parameters.json-dist)
to contain only the absolute minimum[^minimum] number of
parameters. These are the parameters that are necessary for every project that
utilizes the Nubis project.

| Parameter     | Description |
|---------------|-------------|
|ServiceName    | Name of service from [here](https://inventory.mozilla.org/en-US/core/service/)
|Environment    | Sandbox or Dev or Prod
|SSHKeyName     | Name of AWS ssh key to install on ec2 instances
|TechnicalOwner | Email address or distribution list
|AmiId          | ID output from nubis-builder

[^minimum]: Well, not really since technically the environment can be discovered.

### Credentials

When deploying your stack using the [AWS cli tools](http://aws.amazon.com/cli/)
you will be using an API keypair. You will need to take extra precaution to
ensure that these secrets remain, well, secret. This includes dressing up your
.gitignore file, taking care with pastebins and the like.
