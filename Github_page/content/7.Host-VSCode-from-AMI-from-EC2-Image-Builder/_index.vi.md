---
title : "Triển khai VSCode từ AMI build từ EC2 Image Builder"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 7. </b> "
---


EC2 Image Builder giúp tự động hóa việc tạo, quản lý và triển khai các ami “golden” được tùy chỉnh, an toàn và cập nhật được cài đặt sẵn và cấu hình sẵn bằng phần mềm và cài đặt. 

Phần này ta sẽ xây dựng một AMI cũng như phần 5, nhưng bằng cách dùng EC2 Image Builder để tạo ra một quy trình image pipeline từ khâu cấu hình hạ tầng (OS, VPC, subnet, security group), IAM instance profile, tới việc cài đặt cho ami, triển khai thành instance và test trạng thái instance đó, đồng thời scan lỗ hổng bảo mật trong ami.

Tham khảo: [AWS for Microsoft Workloads Immersion Day](https://catalog.us-east-1.prod.workshops.aws/workshops/d6c7ecdc-c75f-4ad1-910f-fdd994cc4aed/en-US)

#### Thuật ngữ

##### AMI
Đơn vị triển khai cơ bản trong Amazon EC2. AMI là VM image được cấu hình sẵn có chứa hệ điều hành và phần mềm được cài đặt sẵn để triển khai các phiên bản EC2.

##### Image Pipeline
Cấu hình tự động để xây dựng hình ảnh hệ điều hành an toàn trên AWS. Image Builder pipeline được liên kết với một image recipe xác định các giai đoạn xây dựng (build), xác thực (validation) và thử nghiệm (test) cho vòng đời xây dựng image. Image pipeline có thể được liên kết với infrastructure configuration nhằm định nghĩa nơi image được xây dựng (build). Bạn có thể xác định các thuộc tính, chẳng hạn như version, subnet, security groups, logging và các cấu hình khác liên quan đến cơ sở hạ tầng.

##### Source Image
Image recipe là một tài liệu (document) xác định source image và các thành phần sẽ được áp dụng cho source image để tạo cấu hình mong muốn cho hình ảnh đầu ra. Bạn có thể sử dụng image recipe để sao chép các bản dựng (build). Image Builder recipe có thể được chia sẻ, phân nhánh và chỉnh sửa bằng console, AWS CLI hoặc API. 

##### Build Components

Tài liệu dàn dựng (orchestration documents) xác định trình tự các bước để tải xuống, cài đặt và cấu hình các gói phần mềm. Chúng cũng xác định các bước xác thực và tăng cường bảo mật. Một thành phần được xác định bằng định dạng tài liệu YAML.