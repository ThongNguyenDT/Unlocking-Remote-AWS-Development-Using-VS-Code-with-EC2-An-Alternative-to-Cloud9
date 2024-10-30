---
title: "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

Phần này ta sẽ triển khai 1 ứng dụng web cho VS Code trên EC2 từ máy ảo Amazon Linux 2 (từ giờ gọi tắt là AL2), rồi kết nối tới trang web đó một cách an toàn qua port forwarding của AWS Session Manager System Manager. Tham khảo:

1. [https://github.com/aws-samples/vscode-on-ec2-for-prototyping](https://github.com/aws-samples/vscode-on-ec2-for-prototyping)
2. [https://github.com/peteragility/ssm-port-forward](https://github.com/peteragility/ssm-port-forward)
3. [Installing Amazon Linux 2 on VMware - Step-by-Step Guide (youtube.com)](https://www.youtube.com/watch?v=3hzIwa-q35E&t=29s)

![image.png](image.png)

## 3.0. Cấu hình SSH trên máy ảo AL2

![Untitled](Untitled%2052.png)

Cho thuận tiện trong việc gõ lệnh, mình cấu hình SSH cho máy ảo AL2 để truy cập từ Terminal máy tính thật.

### Bước 1: Mở file /etc/ssh/sshd_config

`sudo nano /etc/ssh/sshd_config`

### Bước 2: Đảm bảo có dòng “PasswordAuthentication yes”

### Bước 3: Reset lại sshd

`sudo systemctl restart sshd`

## 3.1. Cài AWS CLI

Tham khảo: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

### Linux:

```jsx
curl "[https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### Windows:

Tải và cài AWS CLI từ [`https://awscli.amazonaws.com/AWSCLIV2.msi`](https://awscli.amazonaws.com/AWSCLIV2.msi)

### Kiểm tra phiên bản

```jsx
aws --version
```

## 3.2. Cài NodeJS

- tham khảo [Tutorial: Setting Up Node.js on an Amazon EC2 Instance - AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)

### Bước 1: Cài nvm

![Untitled](Untitled%2053.png)

### Bước 2: Cài NodeJS LTS

![Untitled](Untitled%2054.png)

Phiên bản NodeJS đã cài là 20.16.0.

### Bước 3: Xác định phiên bản bằng lệnh `node -v` thì lỗi không thấy thư viện

![Untitled](Untitled%2055.png)

Để khắc phục vấn đề này, mình tham khảo link [node.js - GLIBC_2.27 not found while installing Node on Amazon EC2 instance - Stack Overflow](https://stackoverflow.com/questions/72022527/glibc-2-27-not-found-while-installing-node-on-amazon-ec2-instance), có 2 giải pháp:

1. Downgrade version của NodeJS (hiện tại là 20.16.0) xuống 16.0.0.

```jsx
nvm install 16.0.0
```

1. Cài phiên bản thư viện GLBIC_2.27 trở lên

Từ link stack overflow trên sẽ dẫn tới link [glibc 2.27+ on Amazon Linux 2 | AWS re:Post (repost.aws)](https://repost.aws/questions/QUrXOioL46RcCnFGyELJWKLw/glibc-2-27-on-amazon-linux-2). Theo đó, ta biết việc nâng cấp phiên bản là không thể, mà downgrade NodeJS sẽ có thể gặp rủi ro thiếu thư viện hỗ trợ bất cứ khi nào web app triển khai ở phía sau được nâng cấp.

## 3.3. Cài session-manager plugin

tham khảo [Install the Session Manager plugin on Amazon Linux 2 and Red Hat Enterprise Linux distributions - AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/install-plugin-linux.html))

### Bước 1: Cài session-managuer-plugin

```jsx
sudo yum install -y [https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm](https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm)
```

![Untitled](Untitled%2056.png)

### Bước 2: Cài đặt project

```jsx
npm ci
```

![Untitled](Untitled%2057.png)

### Bước 3: Nếu chưa từng sử dụng CDK trước đây, quy trình Bootstrap chỉ cần thiết cho lần đầu tiên. Lệnh sau không cần thiết nếu đã bootstrap.

```jsx
npx cdk bootstrap
```

![Untitled](Untitled%2058.png)

### Bước 4: Triển khai lên AWS:

```jsx
npx cdk deploy
```

![Untitled](Untitled%2059.png)

Nếu được hỏi “Do you wish to deploy these changes?” Nhập y

![Untitled](Untitled%2060.png)

Chờ vài phút để cài đặt:

![Untitled](Untitled%2061.png)

### (phụ): Tiến hành quan sát

![image.png](image.png)

![Untitled](Untitled%2062.png)

### stack VscodeOnEc2ForPrototypingStack

Tag Output:

![Untitled](7d1990f3-5267-4d61-956a-f43a5d2176e1.png)

Tag Resources:

![Untitled](ffa21354-6be4-459d-9dff-d82498bf9920.png)

![Untitled](Untitled%2063.png)

### CDKToolkit stack

### Tag Outputs

![Untitled](f87f2199-e440-495e-add9-a1ad2ecc9b9f.png)

### Tag Resources

![Untitled](c390728e-dae0-4cba-a1da-8b7d6ba49e44.png)

Check instance

![Untitled](Untitled%2064.png)

Check security group của EC2 instance:

![Untitled](Untitled%2065.png)

Check IAM instance profile:

![Untitled](Untitled%2066.png)

### Bước 5: Kết nối nhưng lỗi

![Untitled](Untitled%2067.png)

Không thể kết nối web app do chỉ mở ở localhost. Nhưng nếu tạo session kết nối EC2 bằng Windows (tham khảo [https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-remote-port-forwarding](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-remote-port-forwarding)):

![Untitled](Untitled%2068.png)

Tada:

![Untitled](Untitled%2069.png)

## 3.4. Demo network performance:

[https://www.youtube.com/watch?v=Pe1Whz-Q0TQ](https://www.youtube.com/watch?v=Pe1Whz-Q0TQ)

## 3.5. Cleanup:

![Untitled](Untitled%2070.png)

Chờ vài phút:

![Untitled](Untitled%2071.png)

Vào S3 Console để xóa S3 bucket đã tạo (tên bắt đầu là cdk-hnb)