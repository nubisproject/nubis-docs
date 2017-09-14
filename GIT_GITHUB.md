# Recommended Practices for Git & GitHub

This document will walk you through some best practices that we recommend for
working with the Nubis project. I will not cover much about the basic operation
of git or GitHub as there are a large number of tutorials online that cover
these topics. Instead I will concentrate on the specifics that will help you to
get the most out of the Nubis project, but most importantly will help you to
avoid some pitfalls along the way.

## Deployment Repository

Lets start with what we will call the "Deployment Repository". This is a git
repository, typically available on GitHub, that contains all of the pieces
necessary to deploy your Application. This includes two things, your application
code and a collection of Nubis files. It is important that you follow this
layout as our automation tools expect to find things in specific locations. For
an example, check out the example [nubis-mediawiki](https://github.com/Nubisproject/nubis-mediawiki)
repository.

### Application Code

Your application code can be contained within this repository or it can simply
be included as a git submodule. The choice to embed your code directly in the
repository simplifies your application by having everything in one location. On
the other hand if you separate out your application code from your deployment
repository you can have different people responsible for different aspects of
your code. Additionally this allows your application code repository to remain
deployment agnostic. The choice is yours, but I generally recommend you use the
submodule method for clean separation of responsibility and technology.

### Nubis Files

The nubis files are all contained in a single folder called, not surprisingly,
nubis. Typically there will be three or more folders contained within the nubis
folder; puppet, builder and cloudformation. You can learn more about this layout
over in the [Nubis Overview](link) document.

## Branching

We recommend using topic (feature) branches while developing new features. This
allows you to switch easily between different development work-flows without all
that stashing nonsense. You can learn more about branching (and merging) [here](http://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging).

## Issues & Pull Requests

For anything related to the Nubis Project itself, you can either file an [issue](https://guides.github.com/features/issues/)
for us or submit a patch by using GtiHubs [pull request](https://help.github.com/articles/using-pull-requests/)
method. Either way we will be sure to work with you to solve your issue.

You might be interested in checking out [Hub](https://hub.github.com/), it makes
working with GitHub from the command line a snap.

## Code Reviews

For everything relating to the Nubis Project itself, we require a code review
before landing anything. We do not currently have a strict process for this,
however one of the core maintainers (or module owner) must review code before it
is merged to any production branch. We define a production branch as any branch
that can affect any running systems. For example, branches that deploy the
Sandbox are considered "Production" as they affect the productivity of people
using this system.

We recommend that you adopt a code review process for all aspects of your
application. This helps to reduce production downtime, helps to maintain
cohesiveness and ensures code continues to follow your style guidelines.

---

## TODO

* Describe versioning
* Details on directory layout may be in another doc and should be linked here.
