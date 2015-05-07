# Nubis - Puppet Best Practices

We use packer (http://packer.io) to build our system images, but under the hood, we use puppet-masterless to do the heavy lifting.

First, let's be clear, you do not have to use puppet at all to build images, you could simply rely on packer's provisionners, copying files to the image and running arbirary shell commands.

However, that is not the recommended mechanism to deliver quality Nubis images.

Keeping to just one tool like puppet keeps things much cleaner. But, being the declarative system that it is, puppet makes it a much better tool to express *intent* than a collection of shell scripts can do.

We are trying very hard to keep our image projects maintainable, and upgradeable, and sticking to simple puppet reciepes and good puppet modules will help us achieve that.

## nubis/puppet/main.pp

This is your project's starting point for Puppet. *nubis-builder* will detect the presence of a <nubis/puppet> folder in your project and automatically upload its content onto the provisionning instance and invoke puppet-masterless with your main.pp as the manifest to apply.

We recommend you keep that *main.pp* manifest to only *include* statements of other manifests in your nubis/puppet folder, keeping components separated.

## nubis/puppet

You can put as many manifests as you want in there, but we recommend you try and group them by logical components. If the application you are bulding requires a webserver, a smtp server and an api service, why not structure it like:

 nubis/puppet/main.pp
 nubis/puppet/webserver.pp
 nubis/puppet/smtp.pp
 nubis/puppet/api.pp

This way, you keep things nice and separated. If you discover that your webserver manifest needs to know something about the smtp manifest, try and not refer from one to the next. Instead, use the main.pp manifest to glue them together, passing values in and out of them, if necessary.

## puppet-modules

You have access to many puppet modules when building Nubis projects, they are baked into the base images and made avaialble automatically when you build images of your own.

For the most up-to-date list of modules available, see https://github.com/Nubisproject/nubis-puppet/blob/master/Puppetfile

If you believe you could benefit from a new puppet module, or a newer version of an included one, just head over to https://github.com/Nubisproject/nubis-puppet and file an issue.

## nubis/Puppetfile

You are allowed to create your own *Puppetfile* in your project, and *nubis-builder* will use *puppet-librarian* to include that puppet module in the image you are building.

However, this is not recommended for production. This will just copy a new module on top of the baked-in modules in the base images, and could cause conflicts and mess up dependencies for other modules.

It's a quick and dirty way to test if an upgrade to a puppet module will work for you, and also a good way to try out a new module for possible inclusion in nubis-puppet.

But otherwise, stay away from that if you can.

## Multi-OS puppet

Right now, Nubis supports Amazon Linux & Ubuntu as base OSes, but that could change.

If you are writing complex puppet code, try and keep it OS agnostic where possible. And if not, keep this in mind and ensure your puppet code will work for all the supported OSes.

## Use existing modules

Try and not reinvent the wheel, and make good use of existing puppet modules. They are great time savers, and shrink the amount of puppet-foo we need to support and maintain.

If you find a module that almost does what you want, but not quite, consider modifiying the module itself and submitting it back upstream instead.

## latest vs. present

This is an area of debate, actually. The purely declarative approch would advocate whenever installing a package, you should describe precisely which version of it you expect.

The pragmatic would tell you to just stick to 'latest' and this way, your images are always up to date.

Which is it? Well, it is really up to you, for now, but there is one recommendation.

Packages you rely very heavily on, consider pinning them down to explicit versions, but make it part of your workflow to test and upgrade that version on a regular schedule that fits with your release cycle.

Packages you need present, but otherwise don't care about them much (i.e. I need *unzip* installed), makr them as 'latest', this way, everytime you rebuild your images, you get the newest versions automatically.

But whatever you chose to do, document it in your project's and revisit from time to time.
