---
title: "Cài đặt môi trường"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1 </b> "
---
Topology:
![topo](/images/img_sec7/1/topo.png)
#### Tạo VPC

Yêu cầu: 

- Private subnet để đặt EC2 instance (tối thiểu 1)
- 1 NAT Gateway để instance truy cập internet
- Tối thiểu 1 Public subnet để đặt Internet Gateway

Để tiện thì dùng tính năng “VPC and more” trong “Create VPC”:
![console](/images/img_sec7/1/1/1.png)

Resource map:
![console](/images/img_sec7/1/1/2.png)
Kết quả:
![console](/images/img_sec7/1/1/3.png)
#### Tạo Role và Policy cho SSM

1. Tạo policy
Tạo Policy dạng JSON, có thể đặt tên là VSCodeBastionHostInstanceRoleDefaultPolicy
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
Kết quả:
![console](/images/img_sec7/1/2/1.png)
2. Tạo role và thêm policy vừa tạo vào role
Đặt tên role là VscodeOnEc2ForPrototyping
![console](/images/img_sec7/1/2/2.png)
Role được dùng cho IAM instance profile

#### Tạo security group cho EC2

Nếu EC2 instance của bạn không phải một ứng dụng công khai, thì theo best practice, không cần inbound rule, vì ta chỉ truy cập tới EC2 instance qua port forwarding của Session Manager.

Đặt tên security group là VSC-SG
![console](/images/img_sec7/1/3/1.png)

