---
title: "Tạo AMI từ instance và chạy instance từ AMI đó"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

(mở rộng của mục 3: không cần dùng CDK)

![image.png](/images/img_sec5/image.png)

## 5.1. Cài đặt môi trường

### 5.1.1. VPC

Yêu cầu: 

- Private subnet để đặt EC2 instance (tối thiểu 1)
- 1 NAT Gateway để instance truy cập internet
- Tối thiểu 1 Public subnet để đặt Internet Gateway

Để tiện thì mình dùng tính năng “VPC and more” trong “Create VPC”:

![Untitled](/images/img_sec5/untitled%2072.png)

Resource map:

![Untitled](/images/img_sec5/untitled%2073.png)

Kết quả:

![Untitled](/images/img_sec5/untitled%2074.png)

### 5.1.2. Tạo Role và Policy cho SSM

### Bước 1: Tạo policy

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

![Untitled](/images/img_sec5/untitled%2075.png)

### Bước 2: Tạo role và thêm policy vừa tạo vào role

Đặt tên role là VscodeOnEc2ForPrototyping

![Untitled](/images/img_sec5/untitled%2076.png)

Role được dùng cho IAM instance profile

### 5.1.3. Tạo security group cho EC2

Nếu EC2 instance của bạn không phải một ứng dụng công khai, thì theo best practice, không cần inbound rule, vì ta chỉ truy cập tới EC2 instance qua port forwarding của Session Manager.

Đặt tên security group là VSC-SG

![Untitled](/images/img_sec5/untitled%2077.png)

![Untitled](/images/img_sec5/untitled%2078.png)

## 5.2 Triển khai

### 5.2.1. Tạo EC2 instance

### Bước 1: Chọn AMI là Amazon Linux 2023, instance type là t3.medium

![Untitled](/images/img_sec5/untitled%2079.png)

### Bước 2: Chọn VPC vừa tạo → private subnet bất kỳ

### Bước 3: Đảm bảo không có public IP

### Bước 4: Chọn security group VSC-SG

![Untitled](/images/img_sec5/untitled%2080.png)

### **Chọn AMI là Amazon Linux 2023, instance type là t3.medium**

## Bước 5: Cấu hình volume 100GB

![Untitled](/images/img_sec5/untitled%2081.png)

### Bước 6: Chọn IAM instance profile tên “VscodeOnEc2ForPrototyping” đã tạo từ trước

![Untitled](/images/img_sec5/untitled%2082.png)

### Bước 7: Mục user-data, dán đoạn code sau:

```
#!/bin/bash
#!/bin/bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]
name=Visual Studio Code
baseurl=https://packages.microsoft.com/yumrepos/vscode
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'

# https://github.com/amazonlinux/amazon-linux-2023/issues/397
sleep 10

sudo yum install -y code git
sudo tee /etc/systemd/system/code-server.service <<EOF
[Unit]
Description=Start code server

[Service]
ExecStart=/usr/bin/code serve-web --port 8080 --host 0.0.0.0 --without-connection-token
Restart=always
Type=simple
User=ec2-user

[Install]
WantedBy = multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable --now code-server

# Install Node.js
sudo -u ec2-user -i <<EOF
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source .bashrc
nvm install 20.11.0
nvm use 20.11.0
EOF
```

Mục đích: triển khai VS-Code trên web port 8080 của EC2 instance

### Bước 8: Cuối cùng, chọn “Create instance”.

Sau vài phút, đảm bảo rằng instance đã được khởi tạo thành công

![Untitled](/images/img_sec5/untitled%2083.png)

### Bước 9: Kết nối instance bằng việc chọn Connect → Session Manager

Đảm bảo SSM agent đã được cài đặt bằng cách kết nối vào instance qua Session Manager

![Untitled](/images/img_sec5/untitled%2084.png)

Khi cấu hình thành công:

![Untitled](/images/img_sec5/untitled%2085.png)

### Bước 10: Kiểm tra web app đã triển khai thành công bằng lệnh

 

```
curl localhost:8080
```

Khi web đã triển khai, ta có thể lấy được tài nguyên của trang đang mở trên port 8080:

![Untitled](/images/img_sec5/untitled%2086.png)

### 5.2.2. Truy cập VS Code triển khai trên EC2 từ SSM qua port forwarding

Thay <instance-ID> và <Private-ID> trong đoạn script dưới tương ứng với private IP và instance ID của EC2 instance

### Kết nối từ Linux (lưu ý phải có GUI trên máy Linux):

```
aws ssm start-session \
    --target <instance-ID> \
    --document-name AWS-StartPortForwardingSessionToRemoteHost \
    --parameters '{"host":["<Private-IP>"],"portNumber":["8080"], "localPortNumber":["8080"]}'
```

![Untitled](/images/img_sec5/untitled%2087.png)

### Kết nối từ Windows:

```
aws ssm start-session ^
    --target <instance-ID> ^
    --document-name AWS-StartPortForwardingSessionToRemoteHost ^
    --parameters host="<Private-IP>",portNumber="8080",localPortNumber="8080"
```

![Untitled](/images/img_sec5/untitled%2088.png)

Khi này, truy cập [localhost:8080](http://localhost:8080) trên browser:

![Untitled](/images/img_sec5/untitled%2089.png)

Kiểm tra performace:

Link youtube: [link Youtube](https://youtu.be/6DxtEsX6gvY)