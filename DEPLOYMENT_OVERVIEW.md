# Deployment Overview
A Nubis Account Deployment consists of a number of standard services and security integrations. This document provides and overview of the account and services provided. Each service is self-contained and links are provided to each services' documentation which details that specific service.

## Nubis Account Diagram
![Nubis Account Diagram](media/Nubis_Account_Diagram.png "Nubis Account Diagram")

### Services Provided
This is a list of all of the services available in a Nubis Account.

**TODO**: Add missing documentation links

 - VPC
 - [Consul](https://github.com/nubisproject/nubis-consul/blob/master/README.md#consul-deployment)
 - Jumphost
 - Fluent
 - Opsec
 - CI
 - VPN
 - User Management
 - CloudTrail
 - NAT / Proxy
 - Prometheus
 - ELK

It is important to note that not all services are deployed in every account. To determine which services are deployed in a specific account you will need to consult the deployment configuration file for that account. For example, you can find the configuration files for the Nubis' Teams accounts in the [nubis-accounts-nubis](https://github.com/nubisproject/nubis-accounts-nubis) repository. NOTE: You will need the decryption keys to view these files. Within each configuration file are a set of feature flags, these flags are used to enable or disable specific services and are discussed [below](#feature-flags).

### Security Integrations
There are a number of security integrations deployed into a Nubis Account. These are not available via feature flags and are always deployed in an account. Note that specific services contain additional security integrations which are detailed with the documentation for the service.

**TODO**: List security integrations

### Feature Flags
Within the account deployment variables file are a number of feature flags. These flags are used to select which services to deploy into the account. For a complete list of services depoyed into a particular account you will need to consult that accounts variables file. Here is an example of some of the feature flags available:

```bash
features.consul = 1
features.jumphost = 1
features.fluent = 1
features.opsec = 1
features.ci = 0
features.stack_compat = 0
features.vpn = 0
features.user_management_consul = 0

```

## Deployment Workflow

**TODO**: Describe workflow in two parts. This diagram is really about application deployments and should be replaced with information specific to account deployments. Application deployment documentation may need to be relocated, but I am not sure ATM.

![Nubis Deployment Workflow](media/Nubis_Deployment_Workflow.png "Nubis Deployment Workflow")
