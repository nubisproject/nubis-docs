# Nubis Cheet Sheet

This document is here to provide convenient reminders of commands for working
with Nubis. You should read the full documentation before using these commands
as many of them require additional setup or configuration steps.

## Working in a sandbox account

Prior to running these commands you should consult the [prerequisites doc](https://github.com/nubisproject/nubis-docs/blob/develop/PREREQUISITES.md)

### Setup the shell

```bash

ACCOUNT_NAME='nubis-training'; ACCOUNT_NUMBER='517826968395'; LOGIN='<login>'

alias vault-exec="aws-vault exec ${ACCOUNT_NAME}-admin --"

NUBIS_DOCKER=( 'docker' 'run' \
                '-u' "$UID:$(id -g)" \
                '--interactive' \
                '--tty' \
                '--env-file' "$(echo ~)/.docker_env" \
                '-v' "$PWD:/nubis/data" )

```

### Building an AMI

```bash

vault-exec "${NUBIS_DOCKER[@]}" -e GIT_COMMIT_SHA=$(git rev-parse HEAD) nubisproject/nubis-builder:v0.7.0

```

### Deploying an application

```bash

sshuttle -v --dns -r \
    <username>@jumphost.stage.us-west-2.nubis-training.nubis.allizom.org \
    10.164.19.0/24

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.4.0 plan

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.4.0 apply

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.4.0 destroy

```
