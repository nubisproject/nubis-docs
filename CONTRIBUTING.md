# Nubis - Contributing

The Nubis project is an open-source, collaborative project. And anybody is more than welcome to contribute to it.

## Prerequisites
Before you can contribute to the Nubis project, you'll need to make sure of a few things beforehand. Head over and read the [Prerequisites](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md) doc first.

## Overview

At this point, you should have all the tooling necessary to make changes to Nubis itself.

Take the time to read the contents of [nubis-docs](https://github.com/Nubisproject/nubis-docs) where you'll find tons of useful documentation explaining a lot more details about the various parts that make up the Nubis project.

### Code standards
 - AWS deployments *must* be written in [Terraform](https://www.terraform.io/)
  - Exceptions will only be allowed for unsupported resources
 - Small utility scripts *must* be written in [Bash](https://www.gnu.org/software/bash/).
  - Bash code *should* be POSIX compliant
  - Bash code *should* be run through [ShellCheck](http://www.shellcheck.net/) before submittal.
 - Tools *must* be written in [Go](https://golang.org/).
  - Go code *should* be run through the [go linter](https://github.com/golang/lint).

### Process

Independently of what you are trying to achieve, the process should be more or less the same.

#### File a GitHub issue

That should always be the first step. You found a bug, you thought of a new feature, you'd like to see something improved, doesn't matter. File an issue with as much information as possible.

This will help keep track of the work being done while at the same time giving better visibility to the rest of the Nubis contributors.

Issues are required for all bugs, features and enhancement. A pull-request will not be merged if it doesn't close an existing issue.

#### Issue Labels

Issues labels are standardized across Nubis repositories and are used to classify the type of work:

 * bug

This is reserved for issues that represent a defect in an existing functionality. Small of big, if something is not behaving as it should, it's a bug.

 * enhancement

This is a request for enhancement to an existing feature. It represent something that is not broken, but an opportunity for improvement.

 * feature

 This is a request for a brand new feature. When requesting that Nubis do something it doesn't do before, it's a feature request.

 * docs

 Issues representing the need to document something, new or old. Improvements to existing documentation, or request for documenting something that currently isn't.

 * question

 Issues that are asking a specific question about Nubis. It could be a request to better explain something, or the begging of a discussion about how to approach a certain problem or possible feature.

 * decision

 Issues marked as questions that result in a concrete decision for the project. This label is used to mark the issue as decided, generally spawning a few more issues for implementation or documentation of what has been decided.

 * upgrade

 Issues reserved for upgrades to external Nubis components included. This could be puppet modules, software packages, etc.
 Generally, these issues will be very log hanging fruits, requiring the bump of a version number somewhere and some testing.

 * invalid

 Default GitHub label used to close issues that are not going to be adressed or are simply invalid.

 * duplicate

 Default GitHub label used to close an issue as a dupliate of another one.

#### Issue Milestones

Milestones are used to track Nubis releases, and **only** repository owners should be allowed to assign them to issues.

Each issue that is slated for inclusion in a particular Nubis release will be assigned to the Milestone that corresponds to that release during the triage and planning process.

#### Fork the appropriate repository

No real work should happen directly on the main Nubis repositories. You should be doing things in a personal fork of these repositories. So fork away, if not something you've already done before.

#### Make a branch

This is one of the ways of GitHub. But it's really sensible.

Every single logical self-contained unit of work should live on a branch for it. Name it in a self-explanatory way, as that name will be shared with others.

Examples of good branch names:

 * add-feature-x
 * fix-time-sync-bug
 * improve-documentation-for-strange-feature-x

Example of bad branch names:

 * documentation
 * fix-bug-1234 (what is that bug again?)
 * stuff
 * work-from-2016-03-03
 * username

#### Do the work

Now you get to do what you've been wanting to do. So go ahead and do it. Fix that bug, improve that feature, add this new knob.

Working in git, commit often, commit soon. But keep in mind that your commit history will possibly be seen and reviewed by others, so keep it tidy if you can.

#### Test the work

No matter how small your changes, you should at a bare minimum ensure you can still build the image with *nubis-builder* before considering your work done.

Depending on what you are doing, you might want to perform much more in-depth testing, by spinning up the image you are building in AWS and such. But do try and make sure your work does what it meant to achieve, and nothing else. If you stumble on a bug or some documentation you'd like to see fixed, start back at the top, and file an issue for that.

#### Submit the work

The Nubis project uses a sheriff system similar to Mozilla's. This means that we try very hard and assign Sheriffs to each component of Nubis, responsible for reviewing changes to that component.

Once you are ready, you should submit a pull-request to the repository you forked, effectively requesting inclusion of your work into Nubis itself.

Remember, you are feeding this to another fellow human who will review your work. Take the time to make the pull-request contain what you think would be the best information necessary to make the job of the reviewer easier. Explain what you are doing, if there are tricky bits, point them out, etc.

The only writes to the official Nubis repositories will be the merging of pull-requests. Feature branches are not mandatory, but highly recommended.

#### Code Review

Every Pull-Request needs to be reviewed (+1) by at least one committer before being allowed to be merged in. This process is still being formalized, and currently relies on the knoledge and experience of the current members of the project.

But, code reviews will at a minimum include these:

 * Code needs to be polished and of acceptable quality
 * Code needs to conform to the projet's coding standards and design principles
 * Code needs to follow a reasonably consistent indentation style
 * Code needs to be atomic ( Each Pull-Request should implement one feature or fix one bug )
 * Whitespace/indentation changes should be handled separately, to keep the noise of the review request down.

Conversation on the pull-requests is encouraged to improve the qualiy of the request prior to merging it. Anybody is welcome to add feedback and/or questions to open pull-requests.

It is the responsability of the submitter of the pull-request to address the issues raised during the review, if they want to see their pull-request successfully merged.

Code is written by people, but it's important to remember that reviews are about the work, not the person. Stay civil and polite, and remember that it's an evaluation of the code and how to help include it in the project.

The objective of any code review, for the reviewer, should be focused on helping the submitter to get his work included into the project.

#### Repeat

At this point, it's almost done. Be prepared for possibly some back and forth with the reviewer. There might be questions about bits of code, for instance.

Or there might be requests for changes or fixes to your work. In that case, it's safe to do that work back on that same branch and they will be added to the pull-request.

Once the review completes successfully, your branch will be merged back into the master branch of the Nubis repository, and your work will be included in the next official image builds.

## Commiters

Each repository has a Commiter Team, whose members are allowed to merge pull-requests into the repository.

*Note*: It's bad practice to merge your own pull-requests, as that defeats the review process.

Adding/Removing a member to that Team is the result of a majority vote among existing members.

(Comitter Agreement and Agreement to follow establish processes, etc)

Each Team also has an appointed Technical Lead that holds a tie-breaking vote on that repository.

Commit access is a privilege, not a right. It's is earned by one's contributions and the quality of the work produced. It's all about the quality and health of the project, nothing less, nothing more.

## Contact

* IRC: ```#nubis-users``` on `irc.mozilla.org`
* General mailing list: [Google Groups] (https://groups.google.com/forum/#!forum/nubis-users)
* Developer mailing list: [Google Groups] (https://groups.google.com/forum/#!forum/nubis-dev)

## TODO: More concrete examples
* Bug Fixes
* Improvements
* New base features
* New stacks
* New services
