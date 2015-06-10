# Nubis - To the cloud, we are going!

## A Design Manifesto, by Mozillians.

## Open by default

This is about everything; code, process, artifacts. The idea here is to treat Nubis as an open-source project, that can (and should) be contributed to by all.

To achieve this, we are hosting *all* code on GitHub, publicly. We are trying to use open-source solutions wherever possible, as our first default. We are attempting to build an infrastructure that anybody should be able to reproduce themselves.

We are also using public puppet modules for as much of the provisioning as possible, contributing upstream when necessary, and forking only when absolutely required. This way, we encourage reuse of public modules, and improve the ones that we find deficient. No more single-use recipes.

The only things contain secrets are the things that *need* to. Amazon credentials, secret keys and things that are *specific* to our deployments of Nubis. There should be no hard coded secrets in puppet modules or elsewhere.

## Dynamic discovery

We have chosen to sidestep configuration management by building machine components designed for dynamic discovery upfront. This leaves the run-time configuration to dynamic discovery instead of a more traditional configuration management system.

This is a very different approach to system design for us. However it has been used before successfully and we believe it is the best approach. When you start thinking about systems as throw-away components, it becomes increasingly difficult to manage and configure them using processes designed for static infrastructure.

If we succeed in this, it will mean that we will be able to create an infrastructure that is capable of adapting to changes in its environment in near-realtime. This will give operators a lot more flexibility and freedom when solving (more interesting) problems up the stack.

## Auto-Scaling by default

One of the things that the cloud provides for is easy deployments, which can provide easy scaling. We want to take full advantage of this possibility by default. This means that the norm will be to make systems auto-scalable and the ones that simply can not auto scale will be the exception.

In some cases this could mean designing systems with a little more complexity than single-system equivalents. However, if we build the right tools and frameworks this increase in complexity will be small and well worth the advantages.

Who doesn't want to run an infrastructure where every component at every layer is able to grow and shrink to adapt to demand? Handling a sudden burst of 10x the usual traffic should be the norm, not the exception.

## Immutable Servers

In the cloud, servers are disposable resources. They can come and go in mere seconds, sometimes outside of our control. One can try and fight this, or one can chose to embrace it. We have decided to fully embrace this very unique feature.

We have chosen to think of individual servers as immutable components. That means building system images that contain everything needed to run a given service, upfront. This also means no software upgrades on running systems and no deployments on running systems. We must learn to think of the servers we run as black boxes into which we have no write capabilities (yes, we know, this is an ideal).

This is another change in the way traditional IT has operated in the past. This will be a learning experience. However, it will force us to think in terms of well defined service components. This will also force us to think hard about what knobs *really* need to be tunable at run-time as opposed to the ones that should be changed through a full build, test and deploy process.

## Reusable by design

Individual systems should aspire to perform one task, and only one task. Ideally doing it really well.

These systems should be designed for efficiency and be as general purpose as possible. We should be able to build, say, one memcache component and builders of systems should be able to use and re-use it for their own purposes, without having to re-engineer it over and over again.

## Decoupled by default

Even if a working system is composed of many components, each of these should have the minimal knowledge possible about all the other components, for example:

* A system should ship its logs off to another system, but should not have to know what is going to be processing those logs at the other end.

* A system should expose telemetry data, but not know what is going to be consuming it, if anything at all.

* A system should expose to others the services it offers, the tunables it recognizes, as well as the services it is looking for.

* A system should discover its own environment and adapt to it dynamically, rather than statically.

This is one of the key concepts that will make reusability possible.

## Isolated by default

Whenever possible, systems should be built and deployed in isolation. Isolation means expecting to be deployed alone, being explicit about external dependencies and externally offered services.

When two different systems need to cooperate to achieve results, we need to ask ourselves if they can be made to operate independently. If so, we need to understand that making them isolated from one another is the ideal goal and that the effort required to achieve this goal is necessary.

## Deployments are the norm, not the exception

Deploying new functionality to an existing system is a normal part of a system's life. However, all too often, this is an unusual process filled with exceptions and special cases. It does not have to be, nor should it be.

From the start of any project, continuous deployments should be the norm. The process a developer uses to quickly deploy and iterate on a project should be able to live with that project throughout its lifetime.

Deploying to production, deploying to a test setup, deploying to a different cloud provider even, should be encapsulated in the exact same process, using the same tools.

Also, from an operator's point of view, deploying a new version of something, whatever it is, should feel safe and identical. A DNS Server or a complex web stack should appear to be the same in terms of deployments and rollbacks.

## Bit for bit repeatability

Most production systems live in multiple copies, sometimes called Development, Staging and Production. Too often these environments claim to be the same, but fail in many subtle and not so subtle ways.

Our ideal is to have 100% bit for bit repeatability between as many parallel environment as are needed. This means we should be running in Production the exact same bits that were tested somewhere else.

This is an ideal that will never be fully achieved, there will always be operational differences between environments. However we should strive to keep these differences as few as possible and ensure they are all known quantities, instead of possible unknowns.

## Testing from the start

Far to often we end up with production systems that have no way to test if they are working properly. For example, loading the main page of a web application does not mean that users can log in.

We intend to build unit tests from the start, which will be used by our continuous integration system to test for real functionality. These tests can be further utilized by being tied into the Production monitoring system to ensure that the application is actually functioning correctly.

## Monitor the things that actually matter

Taken to the extreme we could say that is does not matter how much ram is in use or whether an instance is heavily into swap. What actually matters it whether the system is, say, responsive.

Ideally we will use tests and metrics to monitor the things that actually matter in terms of *usability*. This may include data from many sources, including system resources. We will use this data to inform auto-scaling and tooling to adapt to the changing conditions. Humans will be alerted only when the system can not fix itself in an automated or scripted way.
