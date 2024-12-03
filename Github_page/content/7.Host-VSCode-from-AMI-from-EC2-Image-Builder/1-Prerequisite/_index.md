---
title: "Environment Setup"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1 </b> "
---

Topology:
![topo](/images/img_sec7/1/topo.png)

#### Create VPC

Requirements:

- Private subnet to place EC2 instance (at least 1)
- 1 NAT Gateway for the instance to access the internet
- At least 1 Public subnet to place the Internet Gateway

For convenience, use the “VPC and more” feature in “Create VPC”:
![console](/images/img_sec7/1/1/1.png)

Resource map:
![console](/images/img_sec7/1/1/2.png)

Result:
![console](/images/img_sec7/1/1/3.png)

#### Create Role and Policy for SSM

1. Create a policy
Create a JSON policy, you can name it VSCodeBastionHostInstanceRoleDefaultPolicy

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "*",
                "ec2messages:*",
                "ssm:UpdateInstanceInformation",
                "ssmmessages:*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```


Result:
![console](/images/img_sec7/1/2/1.png)

2. Create a role and add the newly created policy to the role
Name the role VscodeOnEc2ForPrototyping
![console](/images/img_sec7/1/2/2.png)

This role will be used for the IAM instance profile.

#### Create a security group for EC2

If your EC2 instance is not a public-facing application, then according to best practices, there’s no need for inbound rules, as we will only access the EC2 instance via port forwarding through Session Manager.

Name the security group VSC-SG
![console](/images/img_sec7/1/3/1.png)
