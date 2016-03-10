# Versioning
This document describes the versioning standard for the Nubis Project. All repositories and projects that make up part of the "Nubis Platform" *must* conform to this standard.

### Overview
Conceptually the versioning standard in Nubis is quite simple. In practice there are a lot of moving parts that go into successfully implementing the standard. For detailed information on the processes behind implementation take a look at the [releasing](https://github.com/Nubisproject/nubis-docs/RELEASING.md) doc.

We take advantage of [semantic versioning](http://semver.org/) in the form of vn.n.n, where v stands for 'v'ersion. Additionally we have a pre-release (dash) standard.

### Semantic Versioning
We use Semantic versioning without modification where the numbers stand for MAJOR.MINOR.PATCH. We also take advantage of the -PRE_RELEASE option for development and feature preview releases. We have the _BUILD_METADATA option available in cases where it is desirable to append a git hash or similar piece of identifying information.

As a side note, we use an underscore (_) in place of a plus (+) due to limitations in Amazons IAM name spacing. This is the only place where we differ form Semantic versioning.

Example Release Flow:
 - v1.2.0 (Normal Release)
  - v1.2.0-dev
  - v1.2.0-dev_githash
  - v1.2.0-fp1
  - v1.2.0-fp2
  - v1.2.0_githash
 - v1.2.1 (Security Release)
  - v1.2.1-dev
  - v1.2.1_githash
 - v1.3.0 (Normal Release)
  - v1.3.0-dev
  - v1.3.0-fp1

### Patch Release
There are two reasons for a patch release, either we discover regressions after the release or an important security vulnerability is discovered. In either case we will bump the patch segment and release through the normal process.

### Pre Releases
Pre releases are used for development work and feature previews. They take the form of -name with an optional incrementing number (-nameX).

#### Development Release (-dev)
The development dash release (-dev) is the working release. It is incremented immediately after any minor or patch release. For example, once v1.0.2 is released the v1.0.3-dev release will be "cut". In reality this is akin to riding master. In fact most repositories in development for the next point release will be riding master (the -dev release).

The -dev release is unlike other releases in that it is not a "release" in the true sense of the word. This release is not a stable target by any means. It is intended only for development work on the Nubis platform and is "released" multiple times without any notice or incrementing any numbers.

#### Feature Preview Release (-fpX)
Occasionally there is a desire to make a feature available outside of the normal release cadence. This type of release will only be initiated by the Nubis platform team. Feature previews are intended to provide downstream projects time to test and the opportunity to provide feedback before a feature "goes live" during a normal release. There is no schedule for feature preview releases and implementation is entirely optional.

### Additional Notes
 - All libraries *must* be pinned to a version.
 - Utilities *may* be pinned to present or latest.
 - All packer jobs (nubis-builder) *must* be pinned to a release of nubis-base.
  - (ie: source_ami_project_version": "v1.0.2)
 - Dependant projects *must* update their nubis-base version to the current release before releasing.
