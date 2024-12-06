---
title: "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

## Giới Thiệu Workshop: Tích Hợp VSCode với AWS EC2  

Chào mừng bạn đến với workshop thực hành, nơi bạn sẽ học cách tối ưu hóa quy trình phát triển bằng việc tích hợp VSCode với các phiên bản Amazon EC2. Dù bạn cần truy cập phiên bản EC2 riêng tư, chạy VSCode trên đám mây, hay triển khai VSCode trong trình duyệt, workshop này sẽ hướng dẫn bạn các giải pháp thực tế để giải quyết những thách thức trong môi trường phát triển trên đám mây hiện đại.  

### Nội Dung Workshop  
Trong workshop này, chúng ta sẽ khám phá các phương pháp khác nhau để kết nối và triển khai VSCode trong môi trường AWS. Các kỹ thuật này có thể áp dụng cho cả các phiên bản EC2 có hoặc không có địa chỉ IP công khai. Kết thúc workshop, bạn sẽ sở hữu bộ công cụ toàn diện để cải thiện quy trình làm việc của mình bằng cách kết hợp VSCode, EC2 và các dịch vụ AWS khác.  

### Cách Workshop Được Tổ Chức  
Nội dung được trình bày dưới dạng các thử thách, mỗi thử thách tương ứng với một phương pháp mới để tích hợp VSCode với EC2. Bạn sẽ học cách:  
1. **Phát triển từ xa với VS Code:** Khai thác sức mạnh của các tiện ích mở rộng *Remote Development* để kết nối VS Code với EC2.  
2. **Xử lý các EC2 riêng tư:** Làm việc với các instance EC2 riêng bằng cách sử dụng các dịch vụ như Instance Connect Endpoint và AWS Systems Manager.  
3. **Triển khai VS Code dưới dạng ứng dụng web:** Cài đặt và chạy VS Code hoặc Code Server (phiên bản mã nguồn mở của VS Code) dưới dạng ứng dụng web.  
4. **Xây dựng và triển khai AMI tùy chỉnh:** Tạo Amazon Machine Images (AMI) với VS Code được cấu hình sẵn, giúp triển khai nhanh chóng và đồng nhất.  
5. **Bảo mật truy cập từ xa:** Tìm hiểu cách bảo mật truy cập từ xa thông qua CloudFront, đường hầm an toàn (secure tunneling), và nhiều giải pháp khác.  


Cách tiếp cận này giúp bạn học theo từng bước, áp dụng các giải pháp linh hoạt phù hợp với nhu cầu dự án và hạ tầng của mình.  

### Tổng Quan Về Các Thử Thách  

1. **[Giới Thiệu](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/1.introduction/)** Hiểu rõ các yêu cầu và thiết lập môi trường để làm việc với EC2 và VS Code.

2. **[Sử Dụng Remote SSH Extension](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/2.Remote-SSH)** Tìm hiểu cách sử dụng tiện ích mở rộng *Remote SSH* của VS Code để kết nối trực tiếp với các instance EC2.

3. **[Kết Nối VS Code với EC2 Riêng qua Instance Connect Endpoint](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/3.ec2_instance_connect/)** Cấu hình và sử dụng AWS Instance Connect Endpoint để kết nối an toàn với các instance EC2 riêng tư.

4. **[Triển Khai VS Code trên Amazon Linux 2](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/4.Hosting-VS-Code-on-EC2-from-Amazon-Linux-2)** Chạy một phiên bản web của VS Code trên instance EC2 sử dụng Amazon Linux 2.

5. **[Triển Khai VS Code Thủ Công trên EC2](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/5.manually-deploy-vs-code-on-ec2/)** Tự cài đặt và cấu hình VS Code hoặc Code Server trên một instance EC2 để phát triển.

6. **[Tạo AMI từ Instance](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/6.create-ami-from-instance-and-run-instance-from-that-ami/)** Học cách lưu cấu hình bằng cách tạo một AMI từ instance đã thiết lập và khởi chạy instance mới từ AMI đó.

7. **[Triển Khai VS Code bằng EC2 Image Builder](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/7.host-vscode-from-ami-from-ec2-image-builder/)** Sử dụng EC2 Image Builder để tự động hóa quá trình tạo AMI được cấu hình sẵn với VS Code hoặc Code Server.

8. **[Kết Nối VS Code với EC2 Riêng qua AWS Systems Manager](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/8.ssm-connect/)** Khai thác AWS Systems Manager để kết nối an toàn với các instance EC2 riêng mà không cần địa chỉ IP công cộng.

9. **[Triển Khai Code Server trên EC2 với Phân Phối qua CloudFront](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/10.cloudfrontdistribution/)** Cài đặt Code Server trên EC2 và phân phối nó an toàn qua Amazon CloudFront.

### Ai Nên Tham Gia?  
Workshop này phù hợp với:  
- Các lập trình viên và kỹ sư DevOps làm việc với AWS, muốn tối ưu hóa môi trường phát triển trên đám mây.  
- Những người muốn nâng cao kiến thức về các dịch vụ AWS và cấu hình mạng nâng cao.  
- Các nhóm làm việc từ xa cần giải pháp đơn giản hóa việc cộng tác với các thiết lập VSCode.  

Hãy chuẩn bị sẵn sàng để tham gia vào một trải nghiệm học tập thú vị và tương tác. Bắt đầu thôi! 🚀  
