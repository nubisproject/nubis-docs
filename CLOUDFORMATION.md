## Recommended practices for Cloudformation
Cloudformation is a necessary evil when working with AWS. It uses JSON which has a number of staggering limitations. You will soon learn that it is overly rigid in its formatting. Additionally it lacks commenting, which, as you know, is a rather atrocious limitation. In an effort to limit your exposure to JSON we have adopted a nested stack model. Basically you will create a stack template which will use these ready made nested stacks. For an example check out this section of the [nubis-mediawiki template](https://github.com/Nubisproject/nubis-mediawiki/blob/master/nubis/cloudformation/main.json#L70).

### Nested Stacks
Nested stacks are in of themselves simply stacks that you include in a higher level, or container stack. We have created a number of stack templates to cover the most common use cases. You can take a look at them [here](https://github.com/Nubisproject/nubis-stacks). For each stack template we have included a README which includes usage code that you can copy into your stack template. Following the previous example from the nubis-mediawiki project you can see the EC2Stack nested stack template [here](https://github.com/Nubisproject/nubis-stacks/blob/master/ec2.template).

### Stack Outputs
We have created a small [function](https://github.com/Nubisproject/nubis-stacks/blob/master/lambda/LookupStackOutputs/LookupStackOutputs.README.md) that runs in [Lambda](http://aws.amazon.com/lambda/) (an AWS compute service) which makes the outputs of other stacks available for reference in your template. You will find us using this function in nearly every nested stack, sometimes multiple times. While you may not find a need for this in your template it is necessary knowledge for understanding the nested stack templates. For example, in the EC2Stack example above we are calling the function as [VpcInfo](https://github.com/Nubisproject/nubis-stacks/blob/master/ec2.template#L48) and using the VpcId output of the $region-$environment-vpc stack [here](https://github.com/Nubisproject/nubis-stacks/blob/master/ec2.template#L73).

### Parameterization
By utilizing stack outputs we are able to minimize the number of parameters (AWS name for input variables) we need. This simplifies deployments, especially when multiple developers are working on the same project. Back in the nubis-mediawiki project you will find the [parameters.json-dist file](https://github.com/Nubisproject/nubis-mediawiki/blob/master/nubis/cloudformation/parameters.json-dist) to contain only the absolute minimum[^minimum] number of parameters. These are the parameters that are necessary for every project that utilizes the Nubis project.

| Parameter     | Description |
|---------------|-------------|
|ServiceName    | Name of service from [here](https://inventory.mozilla.org/en-US/core/service/)
|Environment    | Sandbox or Dev or Prod
|KeyName        | Name of ssh key to install on ec2 instances
|TechnicalOwner | Email address or distribution list
|AmiId          | ID output from nubis-builder

### Credentials
When deploying your stack using the [AWS cli tools](http://aws.amazon.com/cli/) you will be using an API keypair. You will need to take extra percaution to ensure that these secrets remain, well secret. This includes dressing up your .gitignore file, taking care with pastebins and the like.

[^minimum]: Well, not really since technically environment can be discovered.