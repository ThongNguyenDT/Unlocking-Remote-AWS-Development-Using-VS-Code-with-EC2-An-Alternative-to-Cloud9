---
title: "Hosting VS Code trên EC2 từ Amazon Linux 2 với CDK"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

Phần này ta sẽ triển khai 1 ứng dụng web cho VS Code trên EC2 từ máy ảo Amazon Linux 2 (từ giờ gọi tắt là AL2), rồi kết nối tới trang web đó một cách an toàn qua port forwarding của AWS Session Manager System Manager. Tham khảo:

1. [https://github.com/aws-samples/vscode-on-ec2-for-prototyping](https://github.com/aws-samples/vscode-on-ec2-for-prototyping)
2. [https://github.com/peteragility/ssm-port-forward](https://github.com/peteragility/ssm-port-forward)
3. [Installing Amazon Linux 2 on VMware - Step-by-Step Guide (youtube.com)](https://www.youtube.com/watch?v=3hzIwa-q35E&t=29s)

![Untitled](/images/img_sec4/image.png)

## 3.0. Cấu hình SSH trên máy ảo AL2

![Untitled](/images/img_sec4/untitled%2052.png)

Cho thuận tiện trong việc gõ lệnh, mình cấu hình SSH cho máy ảo AL2 để truy cập từ Terminal máy tính thật.

### Bước 1: Mở file /etc/ssh/sshd_config

`sudo nano /etc/ssh/sshd_config`

### Bước 2: Đảm bảo có dòng “PasswordAuthentication yes”

### Bước 3: Reset lại sshd

`sudo systemctl restart sshd`

## 3.1. Cài AWS CLI

Tham khảo: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

### Linux:

```
curl "[https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### Windows:

Tải và cài AWS CLI từ [`https://awscli.amazonaws.com/AWSCLIV2.msi`](https://awscli.amazonaws.com/AWSCLIV2.msi)

### Kiểm tra phiên bản

```
aws --version
```

## 3.2. Cài NodeJS

- tham khảo [Tutorial: Setting Up Node.js on an Amazon EC2 Instance - AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)

### Bước 1: Cài nvm

![Untitled](/images/img_sec4/untitled%2053.png)

### Bước 2: Cài NodeJS LTS

![Untitled](/images/img_sec4/untitled%2054.png)

Phiên bản NodeJS đã cài là 20.16.0.

### Bước 3: Xác định phiên bản bằng lệnh `node -v` thì lỗi không thấy thư viện

![Untitled](/images/img_sec4/untitled%2055.png)

Để khắc phục vấn đề này, mình tham khảo link [node.js - GLIBC_2.27 not found while installing Node on Amazon EC2 instance - Stack Overflow](https://stackoverflow.com/questions/72022527/glibc-2-27-not-found-while-installing-node-on-amazon-ec2-instance), có 2 giải pháp:

1. Downgrade version của NodeJS (hiện tại là 20.16.0) xuống 16.0.0.

```
nvm install 16.0.0
```

1. Cài phiên bản thư viện GLBIC_2.27 trở lên

Từ link stack overflow trên sẽ dẫn tới link [glibc 2.27+ on Amazon Linux 2 | AWS re:Post (repost.aws)](https://repost.aws/questions/QUrXOioL46RcCnFGyELJWKLw/glibc-2-27-on-amazon-linux-2). Theo đó, ta biết việc nâng cấp phiên bản là không thể, mà downgrade NodeJS sẽ có thể gặp rủi ro thiếu thư viện hỗ trợ bất cứ khi nào web app triển khai ở phía sau được nâng cấp.

## 3.3. Triển khai dự án với CDK

tham khảo [Install the Session Manager plugin on Amazon Linux 2 and Red Hat Enterprise Linux distributions - AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/install-plugin-linux.html))

### Yêu cầu: trước tiên cần cài session-managuer-plugin

```
sudo yum install -y [https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm](https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm)
```

![Untitled](/images/img_sec4/untitled%2056.png)


### Bước 1: clone source code và chuyên thư mục làm việc vào đó

```
git clone https://github.com/aws-samples/vscode-on-ec2-for-prototyping && cd vscode-on-ec2-for-prototyping
```

### Bước 2: Cài đặt project

```
npm ci
```

![Untitled](/images/img_sec4/untitled%2057.png)

### Bước 3: Nếu chưa từng sử dụng CDK trước đây, quy trình Bootstrap chỉ cần thiết cho lần đầu tiên. Lệnh sau không cần thiết nếu đã bootstrap.

```
npx cdk bootstrap
```

![Untitled](/images/img_sec4/untitled%2058.png)

### Bước 4: Triển khai lên AWS:

```
npx cdk deploy
```

![Untitled](/images/img_sec4/untitled%2059.png)

Nếu được hỏi “Do you wish to deploy these changes?” Nhập y

![Untitled](/images/img_sec4/untitled%2060.png)

Chờ vài phút để cài đặt:

![Untitled](/images/img_sec4/untitled%2061.png)

### (phụ): Tiến hành quan sát

![image.png](/images/img_sec4/image.png)

![Untitled](/images/img_sec4/untitled%2062.png)

### stack VscodeOnEc2ForPrototypingStack

Tag Output:

![Untitled](/images/img_sec4/7d1990f3-5267-4d61-956a-f43a5d2176e1.png)

Tag Resources:

![Untitled](/images/img_sec4/ffa21354-6be4-459d-9dff-d82498bf9920.png)

![Untitled](/images/img_sec4/untitled%2063.png)

### CDKToolkit stack

### Tag Outputs

![Untitled](/images/img_sec4/f87f2199-e440-495e-add9-a1ad2ecc9b9f.png)

### Tag Resources

![Untitled](/images/img_sec4/c390728e-dae0-4cba-a1da-8b7d6ba49e44.png)

Check instance

![Untitled](/images/img_sec4/untitled%2064.png)

Check security group của EC2 instance:

![Untitled](/images/img_sec4/untitled%2065.png)

Check IAM instance profile:

![Untitled](/images/img_sec4/untitled%2066.png)

### Bước 5: Kết nối nhưng lỗi

![Untitled](/images/img_sec4/untitled%2067.png)

Không thể kết nối web app do chỉ mở ở localhost. Nhưng nếu tạo session kết nối EC2 bằng Windows. Tham khảo [https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-remote-port-forwarding](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-remote-port-forwarding)

![Untitled](/images/img_sec4/untitled%2068.png)

Tada:

![Untitled](/images/img_sec4/untitled%2069.png)

## 3.4. Demo network performance:

[https://www.youtube.com/watch?v=Pe1Whz-Q0TQ](https://www.youtube.com/watch?v=Pe1Whz-Q0TQ)

## 3.5. Cleanup:

![Untitled](/images/img_sec4/untitled%2070.png)

Chờ vài phút:

![Untitled](/images/img_sec4/untitled%2071.png)

Vào S3 Console để xóa S3 bucket đã tạo (tên bắt đầu là cdk-hnb)
