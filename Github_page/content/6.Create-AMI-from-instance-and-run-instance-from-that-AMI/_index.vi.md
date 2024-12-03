---
title: "Tạo AMI từ instance và chạy instance từ AMI đó"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b> 6. </b> "
---

## 6.1. Tạo AMI

Tham khảo: [https://000004.awsstudygroup.com/5-amazonec2basic/5.3-createcustomami/](https://000004.awsstudygroup.com/5-amazonec2basic/5.3-createcustomami/)

### Bước 1: Chọn Instance. Ô Action, Chọn Image and template → Create Image

![Untitled](/images/img_sec6/44af6854-4387-4be2-b57d-9eec29de282d.png)

### Bước 2: Đặt tên rồi chọn Create Image

![Untitled](/images/img_sec6/untitled%2090.png)

Chờ vài phút tới khi tình trạng OK:

![Untitled](/images/img_sec6/untitled%2091.png)

## 6.2. Tạo instance từ AMI

### Bước 1: Ở tag trái AMIs, chọn AMI vừa tạo, chọn “Launch instance from AMI

![Untitled](/images/img_sec6/untitled%2092.png)

### Bước 2: Đặt tên cho instance tạo từ AMI là `VSCodeFromAMI`

![Untitled](/images/img_sec6/untitled%2093.png)

### Bước 3: Giữ nguyên instance type (muốn nhanh hơn thì để type t3.medium). Mục key pair chọn `Proceed without a key pair` vì ta kết nối bằng SSM.

![Untitled](/images/img_sec6/untitled%2094.png)

### Bước 4: Chọn VPC có private subnet. Phần subnet chọn private subnet.

### Bước 5: Chọn security group tạo ở mục trước, `VSC-SG`, hoặc tạo mới: không có inbound rule; outbound rule mở cho Internet trên mọi giao thức.

![Untitled](/images/img_sec6/untitled%2095.png)

### Bước 6: Gán IAM profile cần thiết cho SSM

![Untitled](/images/img_sec6/untitled%2096.png)

### Bước 7: Chọn Create instance

Sau vài phút, instance sẽ khởi tạo xong

![Untitled](/images/img_sec6/untitled%2097.png)

### Bước 8: Tạo session kết nối instance vừa tạo từ ami qua port forwarding:

```
aws ssm start-session ^
--target instance-id ^
--document-name AWS-StartPortForwardingSessionToRemoteHost ^
--parameters host="private-ip",portNumber="8080",localPortNumber="8080"
```

![Untitled](/images/img_sec6/untitled%2098.png)