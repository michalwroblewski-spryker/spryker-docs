---
title: Environment provisioning
description: This document explains core concepts that are important to understand before filing an environment provisioning request.
last_updated: Mar 14, 2023
template: concept-topic-template

---

Before starting to provision your Spryker PaaS environment we want to explain what we need from you to provision your environment. The environment provisioning is started by creating a case using your support portal access. If you have questions, visit [Spryker Support Portal](https://support.spryker.com). If you do not yet have a support portal access, request one through the [request form](https://www.surveymonkey.com/r/XYK5R26) on SurveyMonkey. 
Once you are logged in to Spryker Support Portal, you can send the [environment provisioning request](https://support.spryker.com/s/hosting-change-requests/change-request-new-environment).

{% info_block warningBox %}

This can only be done through a customer account. To request an environment, partners need to work with their respective customers.

{% endinfo_block %}

{% info_block infoBox %}

The sections marked with asterisk (\*) are mandatory to proceed with provisioning request.
{% endinfo_block %}


The following sections explain what information you need to provide to start provisioning your environment.

## Environment

This section explains how different attributes of your environment are used.

### Environment name

The project name and environment type constitute your environment name. The environment name is referenced in AWS services endpoints:
* Redis
* DB
* Elasticsearch
* Deploy files
* S3 buckets

### Project name\*

A *project name* is the name of the customer or the name of the project that the customer wants it to be called. The project name can't be modified once an environment is provisioned. It must not contain special characters or spaces.

**Recommended:** myshop

**Not recommended:** myshöp, my shop, my-shop, my$shop

### Environment type\*

An *[environment type](/docs/cloud/dev/spryker-cloud-commerce-os/environments-overview.html)* refers to the popular naming convention for environment tiers in software development. You can refer to your contract to what environments you are entitled and choose the respective one—for example:

Lower environments: STAGE, DEV

Production: PROD-LIKE, PROD

**Example:**

If `myshop` is the customer, then `myshop-PROD` is an environment name, where `myshop` is the project name, and `PROD` is the environment type.

### AWS region\*

*AWS region* is where customers want their infrastructure resources to be available. For more information about AWS region available options, refer to AWS official [documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html). For the AWS region you are entitled to use, check your contract. 
As of today the following AWS regions are supported:
- Asia Pacific (Tokyo)
- Asia Pacific (Seoul)
- Asia Pacific (Mumbai)
- Asia Pacific (Singapore)
- Asia Pacific (Sydney)
- Canada (Central)
- Europe (Frankfurt am Main)
- Europe (Stockholm)
- Europe (Milan)
- Europe (Ireland)
- Europe (London)
- Europe (Paris)
- South America (São Paulo)
- US East (Ohio)
- US East (N. Virginia)
- US West (N. California)
- US West (Oregon)


## Code base and boiler plates

This section explains the aspects of the code base and boilerplate you are using, and its impact on the provisioning task.

### Code base\*

Which code base is used? As Spryker offers different business models based on customer requirements, application services related infra setup vary based on the model. You must know which model is used for the respective environment during provisioning. 

{% info_block infoBox "Note" %}

Similar to project names, you cannot switch between these models after the environment is created. Switching means a complete deprovisioning of your environment. You cannnot carry over any data—for example: B2B, B2C, B2B Marketplace, or B2C Marketplace.

{% endinfo_block %}

### Repository\*

Codebase where the customer's Spryker application code is residing. Spryker supports only GitHub, GitLab, and Bitbucket code hosting services. If the customer code base is not ready the Spryker team provisions the environment with the previously chosen demoshop model from the most recent release using GitHub.

GitHub: If the customer is using GitHub, provide a link to the GitHub repository including branch and a valid GitHub token, so code pipelines can access it. Share GitHub token in secure way as per [Spryker recommendations](/docs/scos/user/intro-to-spryker/support/how-to-share-secrets-with-the-spryker-support-team.html).

GitLab and Bitbucket: Connecting GitLab and Bitbucket repositories directly to pipelines is not supported. Therefore, we have to enable codecommit feature during provisioning. Connections with pipelines can be established  only after the environment is provisioned. If possible, grant GitLab or Bitbucket access to the Spryker engineer working on this request. If not, we will use your deploy file along with the Spryker demo shop during provisioning. For detailed information about the connection process, see [Connect a GitLab code repository](/docs/cloud/dev/spryker-cloud-commerce-os/connecting-a-code-repository.html#connect-a-gitlab-code-repository) section in "Connecting a code repository".

{% info_block infoBox "Note" %}

We can share the required credentials mentioned in the preceding documentation only after environment provisioning is complete.

{% endinfo_block %}

### Deploy file\*

The *Deploy file* is a YAML file used by the Docker SDK to build infrastructure for applications. If a customer provides their own repository, a deploy file must be provided. For demo shop deployments, Spryker Cloud Ops prepares the file on their own. A repository usually has multiple deploy files that are relevant for different purposes and environments. Demoshops have a `deploy.dev.yml` file that is mostly meant for local development purposes. The naming of these files is important. The naming convention is `deploy.{PROJECT_NAME}-{ENVIRONEMENT_NAME}.yml`—for example, `deploy.myshop-production.yml`. For reference, see [Deploy file](/docs/scos/dev/the-docker-sdk/{{site.version}}/deploy-file/deploy-file.html). The most relevant deploy file that can be used as reference is [deploy.aws-env-template.yml](https://github.com/spryker-shop/b2b-demo-shop/blob/master/deploy.aws-env-template.yml). Each demo shop repository has its respective deploy file template. Adjust this accordingly to your requirements and preferences following the preceding documentation and share it with Spryker.

## Domains

This section explains how you can choose a domain name for your project. The domain name determines the URL your shop will be available under.

### Domain name\*

A domain name to be set for the environment. If not provided, it is a default non-public Spryker domain—for example, `myshop-production.cloud.spryker.toys`. Domain names can later be changed. However, if you already know a domain for application, you can specify this domain in your `deploy.yml` files. During the provisioning process, we provide you with CNAMES and Validation records that you can set in your DNS management. The validation records let us provide an SSL certificate for you, and the CNAME records point your domain to the public DNS name of the load balancers responsible for your environment, effectively directing visitors to that domain to your Spryker Application.
Please note Spryker is only issuing SSL certificates for endpoints that are managed by Spryker Commerce Cloud.

## User access\*

The following is a list of customer or partner users that must have access:
* AWS Console access: This access can be used to access environment CloudWatch logs, deployment pipelines, parameter store, S3 buckets. Provide the email address of users that need access.
* VPN: This access can be used to access services such as databases, Jenkins, and RabbitMQ. It is usually needed for developers or DevOps personnel. Provide the email address of users that need access.
* SSH: This access can be used to access the Bastion Host, and from there, reach other services via [port forwarding](/docs/cloud/dev/spryker-cloud-commerce-os/access/connecting-to-services-via-ssh.html). This is usually needed in special cases for developers or DevOps personnel. Provide an SSH public key and email address (to use SSH, you require VPN access).
* SFTP: This access is required to SFTP bastion host. Provide an SSH public key and email address (to SFTP it also requires VPN access). Please note SFTP is not provisioned by default.

## Additional Attributes

In the following we want to explain what additional attributes you can specify at the beginning of your provisioning already, as well as accesses you can request.

### SFTP
Spryker implemented SFTP on top of EFS.This can be used in case of any third party integrations or for explicit data-imports via Jenkins jobs if required on project level. Note that SFTP is only available on bastion and Jenkins. This feature is disabled by default. This can also be requested later via support ticket, but it is preferred to validate this option during provisioning.

### Site to Site VPN
A Site-to-Site VPN (Virtual Private Network) is a type of network connection that enables secure communication between two or more geographically separated networks. This type of VPN allows two or more sites to establish a secure and encrypted connection over the internet or other public networks, creating a virtual private network between the two sites.
- customer gateway IP address
- IP ranges on the customer side that would need access to Spryker VPC
Optional:
- Device name on customer gateway
- Custom BGP(Border Gateway Protocol) ASN
- Dynamic BGP routes
- Other Parameters of tunnels

In case IPsec is or will be needed, provide your internal subnet CIDR, so our Spryker VPC does not overlap here. 
It is very important to evaluate this option during provisioning as Spryker can not change this later once the environment is provisioned and the environment will need to be recreated in case an overlap will be identified.

### Default Network Settings
Each Spryker Cloud Commerce environment is using a dedicated VPC (Virtual Private Cloud) 
The default CIDR used for:
- first non-production environment is 10.105.0.0/16.
- first production environment is 10.106.0.0/16.
- Any next environment regardless if it is production or non-production will be in the range 10.107.0.0/16 - 10.200.0.0/16
Default CIDR can be changed based on your request. Please make sure you report it while requesting an environment.

### Clone environment
In case you would like to clone an existing environment and use the predefined setup there (eg. access rights, DB), please mention it in your request.
