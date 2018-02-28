# Nubis Cheet Sheet

This document is here to provide convenient reminders of commands for working
with Nubis. You should read the full documentation before using these commands
as many of them require additional setup or configuration steps.

## Working in a sandbox account

Prior to running these commands you should consult the [getting started guide](https://github.com/nubisproject/nubis-docs/blob/develop/GETTING_STARTED.md)

### Building an AMI

```bash

nubis-ctl build

```

### Deploying an application

```bash

nubis-ctl deploy plan

nubis-ctl deploy apply

nubis-ctl deploy destroy

```

### Lint your code

```bash

nubis-ctl lint

```
