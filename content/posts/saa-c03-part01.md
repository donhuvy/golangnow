---
categories:
  - AWS
tags:
  - AWS
  - SAA-C03
title: "AWS SAA-C03 certificate - Part 01"
date: 2023-05-01T08:27:42+07:00
draft: false
---

## Question 1 (Topic Operating costs and expenditure attribution)

After migrating an on-premises application to EC2, a company is planning the operating system access as the fleet scales. The method must also scale with the number of instances and optimize for operational complexity. Which recommendation would be appropriate to meet the requirement?

A. Use Security Group rules and SSH

B. Use EC2 Serial console

C. Use System Manager Run Command

D. Use System Manager Session Manager


Explain: Scale Elastic Compute Cloud https://docs.aws.amazon.com/autoscaling/application/userguide/services-that-can-integrate-ec2.html

A) https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html
This is functionally equivalent to the on-premises infrastructure, which also does not scales as it requires network connectivity to all instances.

B) https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-serial-console.html
The EC2 Serial Console is only usable on a single instance, acting as a console connection. This may be useful for a non-bootable instance, but does not scale.

C) https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command.html
Run Command can be deployed against an arbitrary number of instances whether they are on-premises or EC2. It also scales with the number of instances.


D) https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html
Session Manager can replace SSH, without the same network connectivity requirements, which scales better, but it is not functionally scalable.


Correct answer: C.

## Question 2

Your company's application is using several custom AWS tags that your accounting department would like to generate reports from. The application runs in serveral AWS accounts that are all part of the same AWS Organization. What action can ensure that the AWS tags are available as part of the AWS monthly bill?

A. Enable User-defined Cost Allocation Tags for each of the AWS tags in the AWS Organizations Management account.

B. Enable User-defined Cost Allocation Tags for each of the AWS tags in all of the AWS Organizations member accounts.

C. Enable AWS-managed Cost Allocation Tags for each of the AWS tags in the AWS Organizations Management account.

D. No action is required, all tags are automatically made available to the Cost Explorer tool and Cost and Usage Reports.


Correct answer: A

Explain:A) AWS tags are not automatically converted into Cost Allocation Tags, and so this task must be done manually, which makes this task not only appropriate, but required.

B) Similar to A, the task must be performed manually, but it is not required for all member accounts, just the Management account.

C) The custom tags are ot available as AWS-managed Cost Allocation Tags, and so this solution would not be plausible.

D) Tags must be enabled as Cost Allocation Tags manually, and so this solution will not work.

See https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activate-built-in-tags.html

Go to https://us-east-1.console.aws.amazon.com/billing/home

In the navigation pane, choose Cost allocation tags: https://us-east-1.console.aws.amazon.com/billing/home#/tags

Under AWS-generated cost allocation tags, choose the createdBy tag: https://us-east-1.console.aws.amazon.com/billing/home#/tags

Choose Activate.

At member account, see https://us-east-1.console.aws.amazon.com/billing/home#/tags

![user_tag.png](/img/2023_05_01_saa_c03/user_tag.png)

![member_view.png](/img/2023_05_01_saa_c03/member_view.png)

## Question 3

Your company has a product which involves data transfer to individual partners. Each partner has unique data, stored in dedicated S3 buckets. Which permissions implementation wold best minimize the risk of any partner accessing inappropriate data?

A. S3 bucket policy applied to each bucket with cross-account permissions

B. S3 bucket ACL + object ACLS with cross-account permissions

C. IAM user in the company account, credentials delivered to partner

D. IAM role in the company account, cross-account trust with partner

Explain: A) The S3 bucket policy is unique to each bucket, and can be customized to allow least-privilege access to a single partner without the possibility of the partner accessing any other S3 bucket.

B) Like S3 bucket policies, bucket ACLs and object ACLs are specific to the scope they are configured to, but do not allow for least-privilege design, as the grantee can only be an entire 12-digit AWS account ID.

C) This solution, while it can be least-privilege, allows for the possibility of "extra" permissions being associated with the IAM user.

D) This solution is similar to C in that it is possible for least privilege, but also possible for extra or unintented permissions.

Correct answer: A

See https://repost.aws/knowledge-center/cross-account-access-s3

## Question 4

You have been asked to fulfill a company requirement to evaluate compliance against individual controls for business continuity, security, PCI, and GDPR. Which AWS services can be used to verify compliance with these controls? (pick two)

A. AWS CloudTrail

B. AWS Lambda

C. AWS Config

D. AWS Detective

E. AWS Security Hub


Explain: A) CloudTrail is an aggregation point for AWS service API logs, but does not act as a mechanism for audits or determining compliance.

B) Lambda functions can be used for mitigation, but not as a specific tool for audits or compliance.

C) Config has dedicated features for determining compliance with built-in or custom controls.

D) Detective can be used for forensic analysis but not for evaluating compliance.

E) Security Hub is an appropriate tool for determining compliance against many different types of controls.

Correct answers: C, E.

## Question 5

An application is deployed to EC2 instances in a private VPC subnet in the `us-west-2` region. There is a functional requirement to access S3 buckets deployed into the `ap-southeast-1` region. There is a security requirement for the end-to-end traffic to remain private. Which combination of steps can meet the functional and security requirements? (pick three)

A. Deploy a VPC Gateway endpoint into the `us-west-2` VPC.

B. Deploy a VPC in the `ap-southeast-1` region with an S3 Interface endpoint.

C. Configure a route table entry in the `us-west-2` VPC to direct S3 traffic to the Gateway endpoint.

D. Configure a VPC peering connection between the `us-west-2` and `ap-southeast-1` VPCs.

E. Configure the application to the use the `ap-southeast-1` region when accessing S3 buckets.

F. Configure the application to use the `ap-southeast-1` Interface endpoint DNS when accessing S3 buckets.

Explain: A, C, E: This may seem like a functional solution that meets the security requirements. Unfortunately, while Gateway endpoints do proxy S3 traffic to the service API endpoint (meeting the security requirement), they cannot accept traffic for buckets deployed in any region outside that of the endpoint.

B, D, F: This solution is more complicated. S3 endpoints, whether they are Gateway or Interface, cannot route traffic to cross-region buckets. Futhermore, Gateway endpoints cannot accept traffic from a remote VPC via a peering connection, so the Interface endpoint is the only functional option. This creates an ENI to accept the S3 traffic, with an associated DNS entry which can be used from the remote VPC. The traffic remains private, meeting the security requirement.

Correct answers: B, D, F.


