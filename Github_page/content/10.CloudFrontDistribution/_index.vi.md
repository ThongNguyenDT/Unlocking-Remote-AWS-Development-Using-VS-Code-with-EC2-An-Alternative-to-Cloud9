---
title: "Hosting Code Server trên EC2 và phân phối qua CloudFront"
date :  "`r Sys.Date()`" 
weight : 9
chapter : false
pre : " <b> 9. </b> "
---

### Tạo VPC
- Yêu cầu: 
  - Private subnet để đặt EC2 instance (tối thiểu 1)
  - 1 NAT Gateway để instance truy cập internet
  - Tối thiểu 1 Public subnet để đặt Internet Gateway

- Sử dụng tính năng “VPC and more” trong Create VPC để tự động tạo các subnet và gateway cần thiết.

![cloudfrontdistribution_1](/images/cloudfrontdistribution/cloudfrontdistribution_1.png)


![cloudfrontdistribution_2](/images/cloudfrontdistribution/cloudfrontdistribution_2.png)

- Resource map:

![cloudfrontdistribution_3](/images/cloudfrontdistribution/cloudfrontdistribution_3.png)

- Khi hoàn thành, VPC và subnet sẽ được tạo sẵn, sẵn sàng để sử dụng.

![cloudfrontdistribution_6](/images/cloudfrontdistribution/cloudfrontdistribution_6.png)

### CloudFormation
Mặc định, template được cung cấp hỗ trợ các khu vực sau:
| Khu vực        | CloudFront Prefix List ID |
|----------------|---------------------------|
| us-west-2      | pl-82a045eb               |
| us-east-1      | pl-3b927c52               |
| ap-southeast-1 | pl-31a34658               |

Để triển khai ở khu vực khác, bạn cần cập nhật bản đồ ID danh sách tiền tố CloudFront. Kiểm tra [tài liệu](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-aws-managed-prefix-lists.html) để biết thêm chi tiết về cách tìm danh sách tiền tố CloudFront theo khu vực.

```
Mappings:
  CloudFrontPrefixListIdMappings:
    us-west-2:
      PrefixListId: "pl-82a045eb"
    us-east-1:
      PrefixListId: "pl-3b927c52"
    ap-southeast-1:
      PrefixListId: "pl-31a34658"
    <YOUR_SELECTED_REGION>:
      PrefixListId: <CLOUDFRONT_PREFIX_LIST_ID_FOR_THE_SELECTED_REGION>
```
Tạo stack mới từ template CloudFormation.

![cloudfrontdistribution_4](/images/cloudfrontdistribution/cloudfrontdistribution_4.png)

![cloudfrontdistribution_5](/images/cloudfrontdistribution/cloudfrontdistribution_5.png)


- Tạo stack:

![cloudfrontdistribution_7](/images/cloudfrontdistribution/cloudfrontdistribution_7.png)


![cloudfrontdistribution_8](/images/cloudfrontdistribution/cloudfrontdistribution_8.png)

- Stack name: ``code-server-stack``
- Ở SubnetID, VPCID: Chọn ID đã được tạo ở trên

![cloudfrontdistribution_9_](/images/cloudfrontdistribution/cloudfrontdistribution_9_.png)

![cloudfrontdistribution_9](/images/cloudfrontdistribution/cloudfrontdistribution_9.png)

- VPCID:

![cloudfrontdistribution_10](/images/cloudfrontdistribution/cloudfrontdistribution_10.png)

- SubnetID:

![cloudfrontdistribution_11](/images/cloudfrontdistribution/cloudfrontdistribution_11.png)

- Nhấn next, submit:

![cloudfrontdistribution_13](/images/cloudfrontdistribution/cloudfrontdistribution_13.png)

![cloudfrontdistribution_14](/images/cloudfrontdistribution/cloudfrontdistribution_14.png)

- Chờ khoảng 5 phút để quá trình tạo tài nguyên hoàn tất.

![cloudfrontdistribution_15](/images/cloudfrontdistribution/cloudfrontdistribution_15.png)

![cloudfrontdistribution_17](/images/cloudfrontdistribution/cloudfrontdistribution_17.png)

- View EC2:

![cloudfrontdistribution_16](/images/cloudfrontdistribution/cloudfrontdistribution_16.png)


![cloudfrontdistribution_18](/images/cloudfrontdistribution/cloudfrontdistribution_18.png)

![cloudfrontdistribution_19_](/images/cloudfrontdistribution/cloudfrontdistribution_19_.png)



### Kết nối instance bằng việc chọn Connect → Session Manager
- Vào EC2, chọn Connect → Session Manager để kết nối với instance.

![cloudfrontdistribution_19](/images/cloudfrontdistribution/cloudfrontdistribution_19.png)

- Kiểm tra web app đã triển khai thành công bằng lệnh:
```
curl localhost:8080
```

![cloudfrontdistribution_20](/images/cloudfrontdistribution/cloudfrontdistribution_20.png)

#### Cấu hình AWS CLI

- Cài đặt và  cấu hình AWS CLI để kết nối với tài khoản AWS của bạn.
- Chạy lệnh cấu hình:
```bash
aws configure
```

- Lệnh này sẽ yêu cầu bạn cung cấp các thông tin sau:

  - **AWS Access Key ID**: Được tạo trên AWS Management Console.
  - **AWS Secret Access Key**: Được tạo cùng với Access Key ID.
  - **Default region name**: Vùng mặc định mà bạn muốn sử dụng (ví dụ: `ap-southeast-1`).
  - **Default output format**: Định dạng đầu ra mặc định (`json`, `text`, hoặc `table`).

#### Cài đặt Session Manager Plugin

- **Windows**:
    1. Truy cập vào [trang tải xuống Session Manager Plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html) trên trang web của AWS.
    2. Tải xuống trình cài đặt dành cho Windows.
    3. Chạy trình cài đặt và làm theo hướng dẫn để hoàn tất cài đặt.

        Tham khảo: [Install plugin windows](https://docs.aws.amazon.com/systems-manager/latest/userguide/install-plugin-windows.html) 

- **MacOS**:
    1. Sử dụng Homebrew để cài đặt plugin:
        
        ```bash
        brew install --cask session-manager-plugin
        ```
        
- **Linux**:
    1. Tải xuống và cài đặt plugin bằng lệnh sau:
        
        ```
        curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
        sudo dpkg -i session-manager-plugin.deb
        ```


#### Xác minh cài đặt

Sau khi cài đặt, bạn có thể xác minh rằng plugin đã được cài đặt thành công bằng cách chạy lệnh sau:
```
session-manager-plugin --version
```

### Truy cập VS Code triển khai trên EC2 từ SSM qua port forwarding

Sử dụng script sau, thay <instance-ID> và <Private-IP> với instance ID và IP của EC2.
```
aws ssm start-session \
  --target <instance-ID> \
  --document-name AWS-StartPortForwardingSessionToRemoteHost \
  --parameters host="10.0.128.88",portNumber:"8080",localPortNumber:"8080"
```

![cloudfrontdistribution_21](/images/cloudfrontdistribution/cloudfrontdistribution_21.png)

- Giao diện VSCode:

![cloudfrontdistribution_22](/images/cloudfrontdistribution/cloudfrontdistribution_22.png)


