# Nubis - Puppet Best Practices

We use [Packer](http://packer.io) to build our system images, but under the hood,
we use puppet-masterless to do the heavy lifting.

First, let's be clear, you do not have to use puppet at all to build images, you
could simply rely on packer's provisioners, copying files to the image and
running arbitrary shell commands.

However, that is not the recommended mechanism to deliver quality Nubis images.

Keeping to just one tool like puppet keeps things much cleaner. But, being the
declarative system that it is, puppet makes it a much better tool to express
*intent* than a collection of shell scripts can do.

We are trying very hard to keep our image projects maintainable, and
upgradeable, and sticking to simple puppet recipes and good puppet modules will
help us achieve that.

## nubis/puppet/main.pp

This is your project's starting point for Puppet. *nubis-builder* will detect
the presence of a <nubis/puppet> folder in your project and automatically upload
its content onto the provisioning instance and invoke puppet-masterless with
your main.pp as the manifest to apply.

We recommend you keep that *main.pp* manifest to only *include* statements of
other manifests in your nubis/puppet folder, keeping components separated.

## nubis/puppet

You can put as many manifests as you want in there, but we recommend you try and
group them by logical components. If the application you are building requires a
webserver, a smtp server and an api service, why not structure it like:

* nubis/puppet/main.pp
* nubis/puppet/webserver.pp
* nubis/puppet/smtp.pp
* nubis/puppet/api.pp

This way, you keep things nice and separated. If you discover that your
webserver manifest needs to know something about the smtp manifest, try and not
refer from one to the next. Instead, use the main.pp manifest to glue them
together, passing values in and out of them, if necessary.

## nubis/puppet/files

If you place files under *nubis/puppet/files*, they will be automatically copied
over the instance, before puppet is invoked.

Once puppet runs, you can access the files you placed in there with the usual
puppet syntax in your puppet .pp files:

    source => "puppet:///nubis/files/my-file"

## nubis/puppet/templates

If you place templates under *nubis/puppet/templates*, they will be
automatically copied over the instance, before puppet is invoked.

Once puppet runs, you can access the templates you placed in there with the
usual puppet syntax in your puppet .pp files:

    source => "puppet:///nubis/templates/my-file"

## puppet-modules

You have access to many puppet modules when building Nubis projects, they are
baked into the base images and made available automatically when you build
images of your own.

For the most up-to-date list of modules available, see this [Puppetfile](https://github.com/Nubisproject/nubis-base/blob/master/nubis/Puppetfile)

If you believe you could benefit from a new puppet module, or a newer version of
an included one, just head over to [nubis-base](https://github.com/Nubisproject/nubis-base)
and file an issue.

## nubis/Puppetfile

You can create your own *Puppetfile* in your project, and *nubis-builder* will
use *puppet-librarian* to include that puppet module in the image you are
building.

This is an excellent way to include a custom module in your build process or to
use a different module than the one that shipd with the *nubis-base* ami. Any
modules you include will overwrite existing modules, this is matched based on
the module (directory) name with no further interrogation.

## Multi-OS puppet

Nubis supports Amazon Linux & Ubuntu as base OSes.

If you are writing complex puppet code, try and keep it OS agnostic where
possible. And if not, keep this in mind and ensure your puppet code will work
for all the supported OSes.

## Use existing modules

Try and not reinvent the wheel, and make good use of existing puppet modules.
They are great time savers, and shrink the amount of puppet-foo we need to
support and maintain.

If you find a module that almost does what you want, but not quite, consider
modifying the module itself and submitting it back upstream instead.

## latest vs. present

This is an area of some debate. The purely declarative approach would advocate
whenever installing a package, you should describe precisely which version of it
you expect. The pragmatic would tell you to just stick to 'latest' and this way,
your images are always up to date. Deciding which way to go is up to your team,
however there are a few things worth considering.

For all components of Nubis we have a policy that all libraries be pinned to
specific versions while utilities can be pinned to latest. We consider this a
fair trade-off between stability and maintainability.

Things you rely very heavily on, consider pinning them down to explicit
versions, but make it part of your work-flow to test and upgrade that version on
a regular schedule that fits with your release cycle.

Things you need present, but otherwise don't care about them much (i.e. I need
*unzip* installed), pin them to 'latest', this way, every time you rebuild your
images, you get the newest versions automatically.

Whichever path you chose you should document it in your project and revisit the
decision from time to time.
