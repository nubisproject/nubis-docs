# IT Walk Through 20150601 Links

## Slide 1

[IT_Walk_Through_20150601.odp](https://github.com/Nubisproject/nubis-docs/blob/master/presentations/IT_Walk_Through_20150601.odp)

## Slide 5

[Aws Credential](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md#aws-credentials)

## Slide 6

[Github Account](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md#github-account)

[Dependencies](https://github.com/Nubisproject/nubis-builder#dependencies)

## Slide 8

[Nubis Dpaste](https://github.com/Nubisproject/nubis-dpaste)

```bash

git clone git@github.com:YOU/nubis-dpaste.git

```

```bash

git submodule update --init --recursive

```

## Slide 10

```bash

nubis-builder build

```

## Slide 11

AmiId: *ami-7bbb844b*

```bash

aws cloudformation create-stack \
--template-body file://nubis/cloudformation/main.json \
--parameters file://nubis/cloudformation/parameters.json \
--stack-name nubis-xxx

```

[Set Up](https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#set-up)

[Create](https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#create)

## Slide 12

```bash

nubis-consul --stack-name nubis-xxx \
--settings nubis/cloudformation/parameters.json get-and-update

```

[Update Consul](https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#update-consul)

## Slide 13

```bash

ssh -A -t ec2-user@jumphost.sandbox.us-west-2.nubis.allizom.org \
"ssh -A -t ubuntu@$(nubis-consul \
--stack-name nubis-xxx \
--settings nubis/cloudformation/parameters.json \
get-ec2-instance-ip)"

```

[Login](https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#login)

## Slide 16

```bash

aws cloudformation delete-stack --stack-name nubis-xxx

```

```bash

nubis-consul --stack-name nubis-xxx \
--settings nubis/cloudformation/parameters.json \
delete

```

[Delete](https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#delete)
