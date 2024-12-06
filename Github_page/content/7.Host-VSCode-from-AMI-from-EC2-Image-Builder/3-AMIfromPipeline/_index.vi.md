---
title: "Tạo instance từ AMI dựng từ pipeline"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3 </b> "
---


#### Tạo AMI từ pipline

1. Chọn pipeline vừa tạo. Ô Action, chọn Run pipeline:

![console](/images/img_sec7/3/1/1.png)

2. Bấm View details để theo dõi chi tiết:

![console](/images/img_sec7/3/1/2.png)
Thấy Image status là Testing. 

3. Chọn link log stream để theo dõi CloudWatch log stream của pipeline


Phase test, step **TestWhetherAppDeploy**:
![console](/images/img_sec7/3/1/3.png)

Phase **RebootTest**:

![console](/images/img_sec7/3/1/3b.png)
Nếu lỗi thì exit code 1, không lỗi thì exit code 0. Sau cùng, instance test không lỗi.

4. Quay lại pipeline đang chạy. Khi build ami xong, image status đổi thành Available. Click vào phiên bản image đã tạo (hình dưới là 1.0.4/1):
![console](/images/img_sec7/3/1/4.png)

5. Quan sát image đã tạo từ EC2 Image Builder. Bấm vào link AMI:
![console](/images/img_sec7/3/1/5.png)
6. Quan sát ami từ trang EC2.
![console](/images/img_sec7/3/1/6.png)
AMI đã tạo thành công, và đã trải qua các test trong quá trình pipeline mà không phát sinh lỗi. AMI đã tạo thành công, và đã trải qua các test trong quá trình pipeline mà không phát sinh lỗi. 

#### Tạo instance từ AMI vừa tạo
1. Vào trang AMI của EC2 console. Chọn ami vừa tạo. Click **Launch instance from AMI**.
![console](/images/img_sec7/3/2/1.png)
2. Trong lúc cấu hình instance, chọn instance type. Để tiết kiệm thì chọn **t2.micro**. 

3. Không cần key pair.
![console](/images/img_sec7/3/2/3.png)
4. Chọn VPC và security group có từ **VSC**, private subnet, đảm bảo không có public IP.
![console](/images/img_sec7/3/2/4.png)
5. Cung cấp IAM instance profile có từ **VSC** để tự động cài đặt SSM agent.
![console](/images/img_sec7/3/2/5.png)
6. Chọn Create instance.

7. Đợi tới khi instance status **Running**.
![console](/images/img_sec7/3/2/7.png)
8. Truy cập web app VSCode bằng Session Manager qua port forwarding.

```
aws ssm start-session ^
--target i-06ae65824244ff79b ^
--document-name AWS-StartPortForwardingSessionToRemoteHost ^
--parameters host="10.0.132.106",portNumber="8080",localPortNumber="8080"
```
![console](/images/img_sec7/3/2/8.png)
Truy cập thành công!