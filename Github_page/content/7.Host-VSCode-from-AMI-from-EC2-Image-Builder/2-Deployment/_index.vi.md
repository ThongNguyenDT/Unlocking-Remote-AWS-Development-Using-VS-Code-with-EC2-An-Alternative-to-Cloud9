---
title: "Triển khai"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2 </b> "
---


#### Tạo Component

1. Truy cập [Components console](https://us-east-1.console.aws.amazon.com/imagebuilder/home?region=us-east-1#/components). Click **Create component**.
![console](/images/img_sec7/2/1/Picture1.png)

2. Chọn Type là **Build**.
![type](/images/img_sec7/2/1/Picture2.png)

3. Chọn các giá trị theo danh sách sau:
- OS: Linux
- Compatible OS Versions: Amazon Linux 2023
- Component name: VSC-Component
- Component version: 1.0.0
![type](/images/img_sec7/2/1/Picture3.png)

4. Mục Definition document, phần Content, copy nội dung sau vào:

```bash
name: HelloWorldTestingDocument
description: This is hello world testing document.
schemaVersion: 1.0

phases:
  - name: build
    steps:
      - name: HostingVSCode
        action: ExecuteBash
        inputs:
          commands:
            - |
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
  - name: validate
    steps:
      - name: VSCodeValidate
        action: ExecuteBash
        inputs:
          commands:
            - |
              #!/bin/bash
              if ! command -v code &> /dev/null
              then
                echo "code could not be found"
                exit 1
              fi

  - name: validate
    steps:
      - name: NodeJSValidate
        action: ExecuteBash
        inputs:
          commands:
            - |
              #!/bin/bash
              if ! command -v node &> /dev/null
              then
                echo "Node could not be found"
                exit 1
              fi

  - name: test
    steps:
      - name: TestWhetherAppDeploy
        action: ExecuteBash
        inputs:
          commands:
            - |
              #!/bin/bash
              if ! sudo lsof -i -n -P | grep 8080 | grep -w code &> /dev/null
              then
                echo "App could not be found"
                exit 1
              fi
```
Nội dung document này gồm 3 phần:
- Build: Dựa trên user-data của phần trước. Cài đặt VS Code dạng web app lắng nghe ở port 8080.
- Validation: Kiểm tra liệu đã cài thành công NodeJS và VS Code
- Test: Kiểm tra liệu web app có tồn tại và lắng nghe ở port 8080.

5. Chọn “Create Component”.
![type](/images/img_sec7/2/1/Picture5.png)
Kết quả:
![type](/images/img_sec7/2/1/Picture5b.png)

6. (Phụ) Tạo version mới bằng cách chọn Action → Create new version.
![type](/images/img_sec7/2/1/Picture6.png)
Chỉ cần đổi nội dung mục content và sửa lại giá trị mục Component version rồi chọn Create component.


#### Tạo role cho EC2 Image Builder

1. Vào trang ["Create Role" của IAM](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/roles/create). Chọn **AWS service**, use case **EC2**.

![type](/images/img_sec7/2/2/1.png)
2. Thêm các policy như hình sau
![type](/images/img_sec7/2/2/2.png)

3. Đặt tên Role là `EC2ImageBuilderInfraConfigRole` và mô tả
![role](/images/img_sec7/2/2/3.png)
#### Tạo pipeline

1. Truy cập [pipeline console](https://us-east-1.console.aws.amazon.com/imagebuilder/home?region=us-east-1#/createPipeline).

2. Đặt tên cho pipeline là **VSC-Pipeline** . Mô tả tùy ý.
![role](/images/img_sec7/2/3/2.png)
Có thể chọn thêm các option **Enable EC2 image scanning** để đảm bảo ami bảo mật. Nhưng để làm được thì phải bật AWS Inspector.

3. Mục Build schedule, chọn **Manual** rồi chọn Next.
![role](/images/img_sec7/2/3/3.png)
4. Mục Recipe, chọn **Create new recipe** . Mục **Image type** chọn **AMI**.
![role](/images/img_sec7/2/3/4.png)
5. Mục General, đặt tên cho ami và phiên bản

6. Mục Base image, chọn **Select managed images** và **Amazon Linux**.
![role](/images/img_sec7/2/3/6.png)
7. Tại Image origin, chọn **Quick start**. Tại Image name, chọn **Amazon Linux 2023 x86**. Chọn version tương thích, hiện tại chọn **Use latest available OS version**.
![role](/images/img_sec7/2/3/7.png)
8. Tại Working directory, gõ `/home/ec2-user` vì sẽ tải nvm vào thư mục home của user thực thi là **ec2-user**.
![role](/images/img_sec7/2/3/8.png)
9. Tại Filter owner mục Components, chọn ô **Owned by me**. Chọn component đã tạo.
![role](/images/img_sec7/2/3/9.png)
Trong hình là phiên bản 1.0.1, vì đã làm thêm bước tạo version mới ở phần trước (nội dung component không đổi).

Mục Test components - Amazon Linux, với mục đích demo, chọn `reboot-test-linux`.
![role](/images/img_sec7/2/3/9b.png)

10. Để mặc định phần Storage rồi Next.
![role](/images/img_sec7/2/3/10.png)


11. Mục Define image creation process, chọn Next.


12. Mục Infrastructure configuration, chọn **Create a new infrastructure configuration**. Đặt tên là `VSC-InfraConfig`. Chọn IAM role đã tạo.
![role](/images/img_sec7/2/3/12.png)

13. Mở rộng mục VPC, subnet and security groups. Chọn instance type là **t3.medium**, VPC là **VSC**, chọn 1 trong 2 private subnet, chọn security group có từ VSC (security không inbound rule của phần trước) để có thể tạo và đánh giá instance. Chọn Next.
![role](/images/img_sec7/2/3/13.png)
Nhắc lại về security group outbound rules:
![role](/images/img_sec7/2/3/13b.png)

14. Mục distribution settings để default rồi Next.
![role](/images/img_sec7/2/3/14.png)

15. Sau khi review, chọn **Create pipeline**.
