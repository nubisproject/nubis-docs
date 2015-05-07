## Walk Through
The process can seem a bit overwhelming and complicated at first. In an attempt to clarify the process for you I have created this little walk through that will allow you to deploy your first application using the Nubis Project.

We will be using the [nubis-mediawiki](https://github.com/nubisproject/nubis-mediawiki) project for our deployment today.

If yo have not already done so I recommend you read the documents linked in [this](https://github.com/Nubisproject/nubis-docs/blob/master/GETTING_STARTED.md#familiarize-yourself-with-the-nubis-project) section of the [Getting Started](https://github.com/Nubisproject/nubis-docs/blob/master/GETTING_STARTED.md) guide.

### Prerequisites
First things first, you need to install a few tools on order to deploy an application using the Nubis Project. This is all covered in our [Prerequisites document](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md).

Next you will need to upload or create an ssh key pair by following the instructions [here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html). You will be able to use this key pair to log into your EC2 instances after they are created.

### Checkout and Deploy
The next thing is to checkout the code and deploy the application. This is covered in the nubis-mediawiki [README](https://github.com/Nubisproject/nubis-mediawiki/blob/master/README.md)

### Play
It is now time to play around with your new deployment. You will need to look up the DNS name of the load balancer in the [AWS web console](https://us-west-2.console.aws.amazon.com/ec2/v2/home#LoadBalancers:). Place that load balancer name in your hosts file along with what you used for ProjectName above like so:

```
www.$ProjectName.sandbox.nubis.allizom.org
```

You should now be able to load the site by placing that url in your browsers url bar.

In order to ssh into your instance you will need to connect through a jumphost. These are in flux at the moment so in the mean time you will need to ping one of us in #infra-aws on irc.mozilla.org for instructions.

Congratulations on your first Nubis deployment. Don't forget to delete your stack once you are done playing around to avoid excess billing charges.