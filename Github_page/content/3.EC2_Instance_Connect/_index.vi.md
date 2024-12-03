---
title: "Kết Nối VSCode với private EC2 thông qua Instance Connect Endpoint"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---


Trong phần này chúng ta sẽ thực hiện kết nối VSCode remote ssh với EC2 nằm trong private subnet

----

### Bước 1: Setup các endpoint cần thiết

Để kết nối ssh với ec2 nằm trong private connect ta cần phải kết nối thông qua một thiết bị trung gian: có thể là **_bastion host_**, hoặc các **_endpoint_** 

Trong phần này ta tiến hành setup với **EC2 Instance Connect Endpoint** 

Bước này chỉ cần thực hiện khi bạn muốn kết nồi **Instance** nằm ở **private subnet**
- Search dịch vụ ***`VPC`***
- Chọn **VPC**
- Chọn tab **Endpoints**
- Chọn **Create endpoints**

![Untitled](/images/EC2_Instance_Connect_Endoint/image_1.png)

Tiếp theo:
- Đặt tên
- Chọn **EC2 Instance Connect Endpoint**
- Chọn VPC
- Bạn có thể hạn ché ip kết nối **Additional settings** -> check box **Preserve Client IP**
- Chọn Security group bạn dùng cho endpoint, nếu để trống, AWS sẽ dùng Default Security group.

    Yêu cầu tối thiểu:
    - Outpound rule cho phép ssh đến EC2
    - Inpound rule của Instance cho phép nhận kết nối ssh từ endpoint
    - Tham Khao thêm cáu hình sg của **EC2 Instance Connect Endpoint**  tại [📦AWS | Doc | Security groups for EC2 Instance Connect Endpoint](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/eice-security-groups.html)
- Chọn subnet, chọn subnet có chứa EC2 Instance của bạn
- Chọn **Create endpoint**

![Untitled](/images/EC2_Instance_Connect_Endoint/image_2.png)

Sau khi quá trình tạo endpoint kết thúc

![image.png](/images/EC2_Instance_Connect_Endoint/image_3.png)

{{% notice tip %}}
♦️ **EC2 Instance Connect** cho phép chúng ta tạo trực tiếp VPC **endpoint** trong phần **kết nối** của ec2 mà không cần pha truy cập dịch vụ **VPC**. \
♦️ **EC2 Instance Connect** không yêu cầu thiết lập thêm các plugin phụ \
♦️ **EC2 Instance Connect** không yêu cầu cấp quyền truy cập để kết nối ec2 vs dịch vụ quản lý vì bản chất nó là kết nối trực tiếp nên không cần dịch vụ quản lý \
♦️ Chỉ có thể kết nối đến một **EC2** cùng lúc. cài đặt cấu hình cho riêng từng máy \
♦️ Cấu hình dễ dàng hơn khi so với **ssm**.
{{% /notice %}}

---

### Bước 2: Thử kết nối
Trở lại với giao diện **EC2** -> **Instance** của bạn

![image.png](/images/EC2_Instance_Connect_Endoint/image_4.png)

Nếu bạn nhìn thấy 3 cảnh báo trên và không thể kết nối được với ec2 của mình trong tab EC2 Instance Connect chứng tỏ các thiết lập về VPC, subnet của bạn hiện tại đang đi đúng hướng.

Chọn Connect with **EC2 Instance Connect Endpoint**

![image.png](/images/EC2_Instance_Connect_Endoint/image_5.png)

Nếu bạn nhận được kết quả nút kết nối màu vàng các thiết lập về sg và endpoint của bạn đã chính xác

![image.png](/images/EC2_Instance_Connect_Endoint/image_6.png)

- Chọn Connect, bản đã có thể kết nối được EC2 Instance của bạn thông qua Endpoint

![image.png](/images/EC2_Instance_Connect_Endoint/image_7.png)

- Nếu không được bạn có thể kiểm tra các thiết lập sau:
  - Security group của bạn có thiết lập đúng không, recommend nếu không chắc chắn bạn nên thiết lập sg default cho cho cả endpoint và EC2 của mình
  - Kiểm tra các endpoint và các instance của bạn có cùng một thiết lập subnet private không. Lưu ý rằng việc chọn subnet cho endpoint ở phần một là thiết lập target để endpoint kết nối đến chứ không phải là vị trí đặt endpoint.
  - Không cần thiết lập endpoint cho public subnet

---

### Bước 3: kết nối bằng CLI

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
  aws ec2-instance-connect ssh  --instance-id <instance_id>
  ```
  Nếu kết quả hiển thị như sau tức quá trình kết nối của bạn đã thành công
  ![image.png](/images/EC2_Instance_Connect_Endoint/image_8.png)

---

### Bước 4: Cấu hình VSCode

Như vậy ta đã thành công kết nối ec2 thông qua cli, bây giờ ta cần  thiết lập chúng kết nối thông qua vscode

[Ta thực hiện các bước tương tự như kết nối với EC2 ở public subnet](/vi/2)

Tại bước edit host ta sẽ thực hiện việc tạo các tunnel để ssh thay vì dùng public ip

Công việc rất đơn giản, chỉ việc cấu hình file host của bạn như sau

```powershell
Host EC2_Instant_Connect_Endpoint
    HostName <your_instance_id>
    User ec2-user 
    IdentityFile "your/part/to/your/key.pem"
    ProxyCommand aws ec2-instance-connect open-tunnel --instance-id %h
```