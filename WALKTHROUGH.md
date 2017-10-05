# Walk Through

The process can seem a bit overwhelming and complicated at first. In an attempt
to clarify the process for you I have created this little walk through that will
allow you to deploy your first application using the Nubis Project.

We will be using the [nubis-dpaste](https://github.com/nubisproject/nubis-dpste)
application for our deployment today.

If you have not already done so I recommend you read the documents linked in
[this](https://github.com/Nubisproject/nubis-docs/blob/master/GETTING_STARTED.md#familiarize-yourself-with-the-nubis-project)
section of the [Getting Started](https://github.com/Nubisproject/nubis-docs/blob/master/GETTING_STARTED.md)
guide.

## Prerequisites

First things first, you need to install a few tools in order to deploy an
application using the Nubis Project. This is all covered in our [Prerequisites document](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md).

## Checkout and Deploy

The next thing is to checkout the code and deploy the application. This is
covered in the nubis-dpaste [README](https://github.com/Nubisproject/nubis-dpaste/blob/master/README.md)

## Play

It is now time to play around with your new deployment. You will need to look up
the DNS name of the load balancer in the [AWS web console](https://us-west-2.console.aws.amazon.com/ec2/v2/home#LoadBalancers:).
Place that load balancer name in your Firefox URL bar and you should see the
dpaste app.

In order to ssh into your instance you will need to connect through a jumphost.

```bash

ssh ec2-user@jumphost1.sandbox.us-west-2.nubis.allizom.org

```

Congratulations on your first Nubis deployment. Don't forget to delete your
stack once you are done playing around to avoid excess billing charges.
