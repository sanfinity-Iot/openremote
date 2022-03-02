At OpenRemote we use AWS for hosting our deployments, the following guide explains how to create and configure AWS hosts for running the OpenRemote stack; it is written from the OpenRemote organisation perspective but can be used to assist in setting up your own AWS hosted infrastructure. Please refer to the AWS documentation for more details on the services/tools mentioned (we don't generally offer AWS support but some kind person may be able to help on the [forum](https://forum.openremote.io).

# AWS services/tools used
To manage the OpenRemote deployments we use the following AWS services/tools:
* Route 53 - Hosted domain zone to allow management of deployment DNS records
* VPC - To allow management of network access to deployments
* EC2 - For management of virtual hosts to host the deployments
* CloudFormation - For provisioning of hosts, networking, etc.
* S3 - Storage of large file data (maps, backups, etc.)
* EFS - To allow mounting large files into hosts without needing EC2 instances with large HDDs

# AWS IAM
This guide does not give details of IAM setup and it is expected that appropriate users, groups, etc. are in place to provide safe access to AWS services following best practices as recommended by AWS.

# AWS management console vs CLI
Interaction with AWS can be done either through the management console UI or using the CLI tool; the UI can be convenient for visualising data or for beginners but the CLI can be more efficient; this guide only discusses the use of the UI to cater for all user levels, it is assumed more advanced users know how to use the CLI to perform the same tasks.

# Signing In
For interactive (UI login) it is recommended to use the [single sign on portal](https://openremote.awsapps.com/start#/), root users should not be used and user accounts should be configured for UI login with the minimal permissions to fulfil their tasks.

For CLI login then an AWS access key ID and secret is needed.

# AWS Region
AWS services are siloed into datacentre regions; this guide focuses on a single region setup specifically `eu-west-1`; in the UI the region can be selected in the top right.

# [EC2 Key Pairs](https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#KeyPairs:)
EC2 SSH private keys are used to allow login to EC2 instances; at least one key pair should be defined.

# [Route 53 Hosted Zone](https://console.aws.amazon.com/route53/v2/hostedzones#)


# Provisioning AWS host
Provisioning an AWS host can be done using one of the included AWS cloud formation template(s) which can be found in the [.ci_cd/aws](../tree/master/.ci_cd/aws) directory.

