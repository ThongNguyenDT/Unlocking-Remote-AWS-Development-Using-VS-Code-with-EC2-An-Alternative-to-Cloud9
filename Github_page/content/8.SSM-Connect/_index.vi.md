---
title: "Kết Nối VSCode với private EC2 thông qua System Manager Endpoint"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 8. </b> "
---


Trong phần này chúng ta sẽ thực hiện kết nối VSCode remote ssh với EC2 nằm trong private subnet

----

### Bước 1: Setup các endpoint cần thiết

Để kết nối ssh với EC2 nằm trong private connect ta cần phải kết nối thông qua một thiết bị trung gian: có thể là **_bastion host_**, hoặc các **_endpoint_** 

Trong phần này ta tiến hành setup với **SSM Endpoint** 

Bước này chỉ cần thực hiện khi bạn muốn kết nồi **Instance** nằm ở **private subnet**
- Search dịch vụ ***`VPC`***
- Chọn **VPC**
- Chọn tab **Endpoints**
- Chọn **Create endpoints**

![Untitled](/images/SSM-Connect/image_1.png)

Tiếp theo:
- Tiến hành tạo bộ 3 ssm endpoint connect như sau:
    - Đặt tên
    - search ***ssm***
    - chọn theo hình
    ![Untitled](/images/SSM-Connect/image_2.png)
  - Tương tự như các bước tạo endpoint ở [EC2_Instance_Connect](/vi/3.ec2_instance_connect/), ta cần phải tạo 3 endpoint bao gồm:
    - com.amazonaws.us-east-1.ssm
    - com.amazonaws.us-east-1.ssmmessages
    - com.amazonaws.us-east-1.ec2messages

Sau khi quá trình tạo endpoint kết thúc

![image.png](/images/SSM-Connect/image_3.png)
![image.png](/images/SSM-Connect/image_4.png)
![image.png](/images/SSM-Connect/image_5.png)
{{% notice tip %}}
♦️ Chúng ta cần trỏ đúng đến VPC, subnet của EC2 cần kết nối \
♦️ Cần mở security group để EC2 và endpoint có thể kết nối được với nhau \
♦️ Cần phải tạo 3 endpoint bao gồm: \
... ♦️ `com.amazonaws.us-east-1.ssm`  
... ♦️ `com.amazonaws.us-east-1.ssmmessages`  
... ♦️ `com.amazonaws.us-east-1.ec2messages`  
{{% /notice %}}
{{% notice tip %}}
♦️ Bản chất SSM là một dịch vụ quản lý nên, bạn **cần cấp quyền** cho phép EC2 Tương tác với dịch vụ này. Và Instance của bạn phải có agent của AWS System manager \
... ♦️ Một số **AMI** mặt định đã có cấu hình agent \
... ♦️ [Bạn có thể tham khảo thêm ở đây](https://docs.aws.amazon.com/systems-manager/latest/userguide/ami-preinstalled-agent.html) \
♦️ Cho phép bạn cấu hình cùng lúc nhiều thiết bị, đây là nội dung của dịch vụ [**AWS Systems Manager**](https://us-east-1.console.aws.amazon.com/systems-manager/home)
{{% /notice %}}

---

### Bước 2: Cấu hình EC2 Instance

Để kết nối session manager bạn phải có gán quyền role cho EC2
#### Tự tạo

##### Tạo policy

Tạo Policy dạng JSON, có thể đặt tên là VSCodeBastionHostInstanceRoleDefaultPolicy

```jsx
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

![image.png](/images/SSM-Connect/image_6.png)

##### Tạo role và thêm policy vừa tạo vào role

Đặt tên role là VscodeOnEc2ForPrototyping

![image.png](/images/SSM-Connect/image_7.png)

Role được dùng cho IAM instance profile

#### Dùng role có sẵn

- Search trên khung tìm kiếm **IAM**
- Chọn dịch vụ **IAM**
- Chọn tab **Role**
- Chọn **Create Role**

![image.png](/images/SSM-Connect/image_8.png)

- Chọn **AWS Service**
- Chọn **EC2**
- Chọn **EC2 Role for AWS Systems Manager**
- Next

![image.png](/images/SSM-Connect/image_9.png)

- **Next**

![image.png](/images/SSM-Connect/image_10.png)

- Đặt tên
- **Create Role**

![image.png](/images/SSM-Connect/image_11.png)

#### Attack role

- Trở về tab **Instances**
- Chọn **Instance**
- Chọn **Action → Security → Modify IAM role**

![image.png](/images/SSM-Connect/image_12.png)

- Chọn role
- Chọn **Update IAM Role**

![image.png](/images/SSM-Connect/image_13.png)

---

### Bước 3: Thử kết nối
Trở lại với giao diện **EC2** -> **Instance** của bạn

![image.png](/images/SSM-Connect/image_14.png)

Tại tab Session Manager nếu bạn nhìn thấy nút kết nối có màu vàng như trong hình có nghĩa là bạn đã thiết lập thành công kết nối của mình

![image.png](/images/SSM-Connect/image_15.png)

Nếu không được bạn có thể kiểm tra các thiết lập sau:

- Security group của bạn có thiết lập đúng không, recommend nếu không chắc chắn bạn nên thiết lập sg default cho cho cả endpoint và EC2 của mình
- Kiểm tra các endpoint và các subnet của mình có trỏ đúng đến cùng một subnet private không. Lưu ý rằng việc chọn subnet cho endpoint ở phần một là thiết lập target để endpoint kết nối đến chứ không phải là vị trí đặt endpoint.
- Không cần thiết lập endpoint cho public subnet
- Kiểm tra xem AMI của bạn có hổ trợ Agent ssm hay không. Nếu có thực hiện  reboot lại instance, nếu không có thể xóa đi, và tạo lại hoặc tiến hành kết nối EC2_Instances_Connect và cài đặt agent

##### Chọn Connect
![image.png](/images/SSM-Connect/image_16.png)
Giao diện kết nối thành công!

---

### Bước 4: kết nối bằng CLI

Các bước tiếp theo yêu cầu mấy phải được thiết lập AWS CLI v2

Nếu chưa từng cài có bạn có thể thực hiện các bước sau:

Tham khảo: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
 - Đối với _Linux_, ta chạy lệnh sau:
    ```jsx
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
    ```
   
 - Đối với _Windows_:
    - Tải và cài AWS CLI từ [`https://awscli.amazonaws.com/AWSCLIV2.msi`](https://awscli.amazonaws.com/AWSCLIV2.msi)
   
 - Kiểm tra lại quá trình cài đặt cli bằng câu lệnh:
    ```jsx
    aws --version
    ```
- Thử kết nối bằng CLI, ta chạy lênh sau:
  ```jsx
  aws ssm start-session --target <instance_id>
  ```
  Nếu kết quả hiển thị như sau tức quá trình kết nối của bạn đã thành công
  ![image.png](/images/SSM-Connect/image_17.png)

---

### Bước 5: Cấu hình VSCode

Như vậy ta đã thành công kết nối ec2 thông qua cli, bây giờ ta cần  thiết lập chúng kết nối thông qua vscode

[Ta thực hiện các bước tương tự như kết nối với EC2 ở public subnet](/vi/2)

Tại bước edit host ta sẽ thực hiện việc tạo các tunnel để ssh thay vì dùng public ip

Công việc rất đơn giản, chỉ việc cấu hình file host của bạn như sau

```powershell
Host EC2_Instant_Connect_Endpoint
    HostName <your_instance_id>
    User ec2-user 
    IdentityFile "your/part/to/your/key.pem"
    ProxyCommand aws ssm start-session --target %h --document-name AWS-StartSSHSession
```