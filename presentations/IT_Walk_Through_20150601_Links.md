## Slide 1: ##
https://github.com/Nubisproject/nubis-docs/blob/master/presentations/IT_Walk_Through_20150601.odp

## Slide 5: ##
https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md#aws-credentials

## Slide 6: ##
https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md#github-account
https://github.com/Nubisproject/nubis-builder#dependencies

## Slide 8: ##
https://github.com/Nubisproject/nubis-dpaste
git clone git@github.com:YOU/nubis-dpaste.git
git submodule update –init --recursive

## Slide 10: ##
nubis-builder build

## Slide 11: ##
AmiId: ami-7bbb844b

aws cloudformation create-stack --template-body file://nubis/cloudformation/main.json --parameters file://nubis/cloudformation/parameters.json --stack-name nubis-xxx

https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#set-up
https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#create

## Slide 12: ##
nubis-consul --stack-name nubis-xxx --settings nubis/cloudformation/parameters.json get-and-update

https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#update-consul

## Slide 13: ##
ssh -A -t ec2-user@jumphost.sandbox.nubis.allizom.org "ssh -A -t ubuntu@$(nubis-consul --stack-name nubis-xxx --settings nubis/cloudformation/parameters.json get-ec2-instance-ip)"

https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#login

## Slide 16: ##
aws cloudformation delete-stack --stack-name nubis-xxx

nubis-consul --stack-name nubis-xxx --settings nubis/cloudformation/parameters.json delete

https://github.com/tinnightcap/nubis-dpaste/blob/master/nubis/cloudformation/README.md#delete
