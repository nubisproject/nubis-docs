

# Nubis - Release Management

This is a document that helps explain all the process involved in Release
Management for the Nubis project. If you are not planning to make a Nubis
release, you can safely ignore this document, unless you are curious.

## Milestones

GitHub milestones are used to track work (issues) against a given Nubis release.
Issues that will be part of that release *must* be assigned to the
corresponding milestone.

### Versioning

The format for releases is documented in the
[versioning](https://github.com/Nubisproject/nubis-docs/VERSIONING.md) doc.

### Code names

Release code-names might be exist, but will be used for purely cosmetic
purposes. The *Happy Panda* release would just be a name for the v1.5.0 release.

### Management

For each Milestone, one of the Tech Leads takes on the Release Manager Hat.

That Release Manager is responsible for triaging what makes it into that
Release, with input from the rest of the team.

Generally, it makes sense for each Milestone to represent a logical and
descriptive amount of work. For example, a Milestone
could be about documentation, bug fixes, implementing a big new feature,
refactoring, etc.

There is no specific defined time limit for a Milestone, but when it's created
and issues triaged into it, it should be
factored in. Milestones that take too much time to complete are a bad practice.
Better to split up work in multiple Milestones,
sequentially reached, instead of one big-bad Milestone.

For that Release, the Release Manager gains a veto, solely for purposed of
tie-breaking project blocking/delaying issues.

The ultimate role of the Release Manager is to successfully complete the
Milestone, with the help of the development team.

## Tags

GitHub tags will be used to make releases of each repository that is part of the
Nubis Project.

Tags *must* follow the format as defined in the [versioning](https://github.com/Nubisproject/nubis-docs/VERSIONING.md)
doc.

All tags *must* be [GPG Signed](https://git-scm.com/book/tr/v2/Git-Tools-Signing-Your-Work)
by the Release Manager. This allows their integrity to be verified.

Each Nubis repository is allowed to follow it's own dash release tagging
schedule, however we encourage them to follow the -dev model as defined in the
[versioning](https://github.com/Nubisproject/nubis-docs/VERSIONING.md) doc.

Major, Minor and Patch releases will be coordinated across all repositories, to
provide a consistent versioning scheme for each Nubis Project release.

## Changelogs

Each repository *must* contain a CHANGELOG.md document in the root directory,
highlighting the changes between releases. It's a very common pattern for
software projects. However, maintaining ChangeLogs can quickly become tiresome.

To address this, all ChangeLogs for Nubis will be generated using
[github_changelog_generator](https://github.com/skywinder/github-changelog-generator)
during the release process.

The process is quite simple and is executed like this:

```bash

github_changelog_generator --future-release v1.0.0 nubisproject/nubis-docs

```

## Cadence

Patch releases are not on any schedule. They are released as soon as work is
completed following notification of a security vulnerability necessitating a
patch release.

Minor releases will be released on a quarterly cadence. They will be released as
close to the end of the quarter as practical. We allow for a delay of up to two
weeks after the quarter, but every effort must be made to keep within this
grace period.

Major releases will occur on an as-needed basis. They are reserved for backwards
incompatible changes and therefore can not be defined in advance. Major releases
will happen organically, as we discover the need as well as define and complete
the milestones for them.

## Announcing

The day-to-day mechanism for communicating with Nubis users is on the
**#nubis-users** channel on irc.mozilla.org

For more official announcements, or announcements that require more reasonable
delivery guarantees, we use the [nubis-announce](https://groups.google.com/d/forum/nubis-announce)
distribution list. It can be reached at [nubis-announce@googlegroups.com](nubis-announce@googlegroups.com)

There will be a formal announcement for all releases sent to the [nubis-announce@googlegroups.com](nubis-announce@googlegroups.com)
distribution list. This announcement will follow the standard template found
[here](https://github.com/Nubisproject/nubis-docs/templates/announce.txt).

## Process

When it comes time to create a release; all pull-requests related to this
release have been merged, the changelog has been generated and all tests have
passed, you are ready to cut a release.

This is the only time you will need to operate on an origin branch directly.
This is different from the normal pull-request based work-flow in that tags can
not be associated with a pull-request. Also this process is described using the
master branch, you may wish to use a feature branch for your release work.

The first thing you will need to do is set up your branches in a manner similar
to the following. That is one branch, named master, which is tracking your fork
and one branch, named originmaster, tracking the master branch from the
nubisproject origin. You can call these whatever you like, however these
instructions will assume you have named them as shown here.

```bash

git checkout -b originmaster --track origin/master
git branch -avv
* master        0645ce9 [tinnightcap/master] Update changelog for v0.9.0-beta1 release
  originmaster  7a8254d [origin/master] Merge pull request #8 from tinnightcap/master

```

Next you will need to ensure that your origin branch is current. Note that all
pull-requests for this release need to have been merged onto the origin prior to
this step. This includes having generated the changelog, committed it, created a
pull-request, having had it code-reviewed and merged. Simply fetch and rebase.

```bash

git checkout master
git fetch origin
git rebase origin/master

```

Now we are ready to create the release using a signed tag as described [above](#tags).
In this case I am creating a beta release for testing in advance of the v0.9.0
release.

```bash
git tag -s v0.9.0-beta -m"Signed beta release for upcoming v0.9.0 release"

```

Push the tag to your fork for testing. You should validate that the release
files (.tar.gz & .zip) are working as expected and that there are no collisions
or typos. You should be able to safely rename or delete tags from your fork,
however once they are pushed to the origin they should no longer be deleted.
You should ensure things are correct prior to continuing on to the next step.
If you have any doubt, have your tags code-reviewed prior to continuing.

```bash

git push --tags

```

Switch to the originmaster branch.

```bash

git checkout originmaster

```

Make sure this branch is up to date.

```bash

git pull

```

Finally push the signed tag to the origin.

```bash

git push --tags

```

That is it. You should verify once more that the release files are correct and
send updates as appropriate.

## Release Order

Due to some interdependencies between various repositories, the order in which
repositories are released has become important. In general you need to release
[nubis-stacks](https://github.com/Nubisproject/nubis-stacks) before you release
any repositories that rely on the nested stacks.

There is a bit of a chicken and egg issue when it comes to releasing [nubis-storage](https://github.com/Nubisproject/nubis-storage).
This is due to the fact that nubis-storage consumes nubis-stacks (requiring a
released nested stack), however the nubis-stacks *storage.template* contains
hard coded ami Ids. The process for solving this is quite simple:

Upload the release ready nested stack templates to the new release directory:

```bash

bin/upload_to_s3 --path "v0.9.0-beta" push

```

Next you need to [edit](https://github.com/Nubisproject/nubis-storage/blob/master/nubis/cloudformation/main.json#L35)
and rebuild nubis-storage:

```bash

cd path/to/nubis-storage
vi nubis/cloudformation/main.json
/StacksVersion
~ update to latest release ~
nubis-builder build

```

Place the generated ami Ids in the nubis-storage [main.json](https://github.com/Nubisproject/nubis-storage/blob/master/nubis/cloudformation/main.json#L76)

```bash

cd path/to/nubis-storage
vi nubis/cloudformation/main.json
/Mappings
~ edit the Mappings with new ami Ids ~

```

Place the generated ami Ids in the [storage.template](https://github.com/Nubisproject/nubis-stacks/blob/master/storage.template#L98)

```bash

cd path/to/nubis-stacks
vi storage.template
/Mappings
~ edit the Mappings with new ami Ids ~

```

Make your pull-request, have it code reviewed and merged:

````bash

git add storage.template
git commit -m"Update storage AMI Ids for v0.9.0-beta release"
git push
hub pull-request -m "Update storage AMI Ids for v0.9.0-beta release"

```

Now you can cut the release of the nubis-stacks repository, making sure you are
up to date first:

```bash

git checkout master
git fetch origin
git rebase origin/master
git tag -s v0.9.0-beta -m"Signed beta release for upcoming v0.9.0 release"
git push --tags

```

Complete the dance, making sure you push the tag to *originmaster* and fetch
back the release ref. This ensures you have locally what is actually in the
release.

```bash

git checkout originmaster
git pull
git push --tags
git checkout master
git fetch origin
git rebase origin/master

```

Finally push the actual release of nubis-stacks to the S3 bucket overwriting
your previous, temporary uploads:

```bash

bin/upload_to_s3 --path "v0.9.0-beta" push

```

You are now in a position to release the remaining repositories (including
nubis-storage). There is generally no order to this process, however there are a
few remaining points.

You MUST test the (currently) three example repositories to make sure they work
prior to releasing them. This is important due to the fact that we are making a
guarantee that if a user chooses to use the project at a known good release,
that this release will be, well, good. What this means is that you need to
actually *nubis-builder build* AND *cloudformation create-stack* on all of the
example repositories followed by some testing. Be sure to update their
respective cloudformation templates to use the newly released nubis-stacks
before you deploy them. The three repositories are:

* [nubis-skel](https://github.com/Nubisproject/nubis-skel)
* [nubis-dpaste](https://github.com/Nubisproject/nubis-dpaste)
* [nubis-mediawiki](https://github.com/Nubisproject/nubis-mediawiki)

That is about all there is to it. You need to close a few issues and send an
announcement to the nubis-announce list, but I am sure you remember all of that
from higher up in this doc. Cheers.
