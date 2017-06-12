
# Nubis - Building Quality Images

Nubis is all about building system images in an automated and repeatable way.

These images should be thought about as immutables ones, that is, images that
are a static known quantity with well known and defined propreties, that only
change when they are built.

A lot of the rational behind that can be found in our
[MANIFESTO](MANIFESTO.md)

There are a few important principles to keep in mind when building good quality
Nubis images.

## Immutable

First and foremost, this is the key proprety of Nubis images. They should be
immutable, baked-in with as much of your project as possible.

This should include dependencies, code, tools, all of it.

The image should contain everything it needs to perform its task from bootup,
without needing to perform special tasks during startup.

For instance, if you have an application with static assets that need to be
post-processed (i.e. minifying javascritp), do that at image build time, not as
part of a bootup migration task.

## Repeatable

When building an image, you can use puppet, and you can use arbitrary shell
commands.

But think of the build process as the build process for some software. You want
to make these steps as explicit and as deterministic as possible, so that if
someone else chooses to rebuild your image at a specific revision, they will get
the same resulting images.

For instance, it's easy to use a shell script to download a tool you need to
your image straight out of GitHub. It's simple, convenient, and a simple *wget*
invocation.

However, if you just grab that tool from the *master* branch, it means it can
change from under you without warning at any time.

Your image might build and function just fine today, but when it gets rebuild
tommorrow with an unrelated change, you'll get a different version of that tool
that might now break your application.

## Distributable

Think of the images you are building as publicly distrutable service components.

Images are public by default, but that's not good enough.

Images should be self-contained and represent a useful piece of functionality.

They should be something that could be useful to someone else as-is.

Don't **ever** include any secrets of any kind, for starters.

But also make sure that there isn't anything specific to your internal project
implementation hard-coded in them.

Things like domain names, usernames, email addresses and the like don't belong
in images. They are piece of configuration, not intergral part of the images.

## Configurable

Take the time to clearly identify the pieces of data the image needs to operate
but can't be baked-in.

Nubis offers mechanism for service discovery ([Consul](CONSUL.md)) and
self-configuration ([confd](CONFD.md)), make use of them.

Amazon Web Services provides the *user-data* mechanism to feed information into
instances at launch time, but we advise strongly against its use.

Every piece of data that is fed to the instance that way, in effect, creates an
API into that instance that must be complied with to be able to launch a working
instance. This would greatly complicate the design of a generic
continuous-integration system, for instance.

This can get quickly out of hands. Add to that the fact that *user-data* can't
be changed once the instance has been started, and you get a very poor
configuration managment system.

## Black boxes

Nubis images, once launched, should be considered like black-boxes that you
can't get access to, apart from it's externally defined interfaces and APIs.

Design your images with this in mind. Use the [Nubis logging mechanism](FLUENTD.md)
if you need to get operational data out of the instances.

Build tools if you need that ability to perform operator tasks of your service,
if you need them.

Use the configuration system to create *knobs* for your service, where it makes
sense.

Want to be able to turn your web application into read-only mode while a traffic
spike is going on? Make that into a configuration parameter.

Want to be able to blacklist certain IPs from your service? Make that into a
configuration list.

Want to be able to enable debugging output for a certain username? Make that
into a user-proprety of the system, or a configuration list.

Assume you'll never have *ssh* access into the instances running in production.

## Absolutely no persistent data

Running instances are disposable assets, and may be killed at any time, replaced
with new fresh copies.

This means that any data that is locally stored on the instance can vanish at
any time.

Do not assume any kind of persistence for the data you store locally on the
instance. If it's important data, **do not** store it locally, hand it off to a
service that's meant for persistency.

Use a database, Amazon RDS, Amazon S3, Amazon EFS, Nubis Storage, ship the logs
away, etc.

AWS does allow for some level or persistency for instance storage, but it should
be avoided as much as possible.
