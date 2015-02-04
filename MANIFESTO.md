Nubis - To the cloud, we are going!

A Design Manifesto, by Mozillians.

* Open by default

This is about everything, code, process, artifacts. The idea here is to treat Nubis as an open-source project, that can (and should) be contributed to all.

To achieve this, we are hosting *all* code on GitHub, publicly. We are trying to use open-source solutions wherever possible, as our first default. We are attempting to build an infrastructure that anybody should be able to reproduce themselves.

We are also using public puppet modules for as much of the provisionning as possible, contributing upstream when possible, and forking only when absolutely necessary. This way, we encourage reuse of public modules, and improve the ones that we find deficient. No more single-use receipes.

The only things kept secrets are the things that *need* to be. Amazon credentials, secret keys, things that are *specific* to our deployments of Nubis.

* Dynamic discovery

We have chosen to sidestep configuration managment, by building machine components upfront, and leaving the run-time configuration to dynamic discovery, instead of a traditional configuration managment infrastructure.

This is a very different approach to system design, for us. But it's been used before successfully, and we believe it's the way to go. When you start thinking about systems as throw-away components, it becomes increasingly difficult to manage and configure them in a way designed for static infrastructures.

If we succceed in this, it will mean we will be able to create an infrastructure that is capable of adapting to changes in its environemnt in near-realtime. Giving the operators of the systems a lot more options and freedom in solving more interesting problems, up the stack.

* Auto-Scaling by default

The cloud means ease of scale and ease of deployment. We do not want to fail at making this a possiblity by default. This means the norm will be to make system auto-scalable, and the ones that just can't be made so will be the exception.

In some case, this could mean designing systems with a little more complexity than single-system equivalents. However, if we build the right tools and frameworks, that increase in complexity should be small and well worth the advantages.

Who doesn't want to run an infrastructure where every component at every layer is able to grow and shrink to adapt to demand? Handling a sudden burst of 10x the usual traffic should be the norm, not the exception.

* Immutable Servers

In the cloud, servers are disposable resources. They can come and go in mere seconds, sometimes outside of our control. One can try and fight this, or one can chose to embrace it. We've decided to fully embrace this very unique feature.

We have chosen to think of individual servers as immutable components. That means building system images that contain everything needed to run a given service, upfront. This means no software upgrades on running systems, no deployments on running systems. We must learn to think of the servers we run as black boxes into which we have no write capabilities (yes, we know, it's an ideal).

This is another change in how traditional IT has been for a long time. This will be a learning experience.

However, this will force us to think in terms of well definied service components. This will force us to think hard about what knobs *really* need to be tunable at run-time as opposed to the ones that should be changed throught a full build, test and deploy process.

* Composable by design

Individual systems should aspire to perform one task, and only one task. Ideally, doing it really well.

These systems should be designed for efficiency and be as general purpose as possible. We should be able to build, say, one memcache component, and builders of systems should be able to use and re-use them for their own purposes, without having to re-engineer them over and over again.

* Decoupled by default

Even if a working system is composed of many components, each of these should have the minimal knoledge possible about all the other components.

A system should ship its logs off to another system, but shouldn't have to know what is going to be processing these logs at the other end.

A system should expose telemetry data, but not know what is going to be consuming it, if at all.

A system should expose to others the services it offers, the tunables it recognizes as well as the services it's looking for.

A system should discover its own environment and adapt to it dynamically rather than statically.

This is one of the key concepts that will make composability possible.

* Isolated by default

Whenever possible, systems should be built and deployed in isolation. Isolation means expecting to be deployed alone, being explicit about external dependencies and externally offered services.

When two different systems need to cooperate to achieve results, we need to always ask ourselves if they would still make sense independently. And if so,
we need to consider the effort in making them isolated from one another as the ideal goal.

* Deployments are the norm, not the exception

Deploying new functionality to an existing system is a normal part of a system's life. However, too often, this is an unusual process, filled with eceptions and special cases. It doesn't have to be.

From the start of any project, continuous deployments should be the norm. The process an initial developer uses to quickly iterate on a system should be able to live with the project thru its lifetime.

Deploying to production, deploying to a test setup, deploying to a different cloud provider, even, should be encapsulated in the exact same process, using the same tools.

Also, from an operator's point of view, deploying a new version of something, whatever it is, should feel safe and identical. DNS Server or a complex web stack should appear to be the same in terms of deployments and rollback to the individual tasked with the task.

* Bit for bit repeatability

Most production systems life in multiple copies, sometimes called Development, Staging and Production. Too often these 3 environments claim at being the same, but fail in many subtle and not so subtle ways.

Our ideal is to have 100% bit for bit repeatability between as many parallel environment as are needed. This means we should be running in Production the exact same bits as what was tested somewhere else.

This is an ideal that will never be achieved fully, there will always be operationnal differences between environments. But this doesn't mean we should strive to keep these differences as few as possible, and ensure they are all known quantities, instead of possible unknowns.

* Testing from the start

* Monitor the things that actually matter


