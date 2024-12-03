---
title: "Deployment"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2 </b> "
---

#### Create Component

1. Go to the [Components console](https://us-east-1.console.aws.amazon.com/imagebuilder/home?region=us-east-1#/components). Click **Create component**.
![console](/images/img_sec7/2/1/Picture1.png)

2. Select the Type as **Build**.
![type](/images/img_sec7/2/1/Picture2.png)

3. Choose the following values:
- OS: Linux
- Compatible OS Versions: Amazon Linux 2023
- Component name: VSC-Component
- Component version: 1.0.0
![type](/images/img_sec7/2/1/Picture3.png)

4. In the Definition document, under the Content section, copy the following content:

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
The content of this document consists of 3 parts:

- Build: Based on the user-data from the previous section. Install VS Code as a web app listening on port 8080.
- Validation: Check if NodeJS and VS Code have been successfully installed.
- Test: Check if the web app exists and is listening on port 8080.

5. Select “Create Component”.
![type](/images/img_sec7/2/1/Picture5.png)
Result:
![type](/images/img_sec7/2/1/Picture5b.png)

6. (Optional) Create a new version by selecting Action → Create new version.
![type](/images/img_sec7/2/1/Picture6.png)
Simply change the content of the component field and update the Component version field, then select Create component.

#### Create role for EC2 Image Builder

1. Go to the ["Create Role" page of IAM](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/roles/create). Select **AWS service**, use case **EC2**.

![type](/images/img_sec7/2/2/1.png)
2. Add the policies as shown in the following image.
![type](/images/img_sec7/2/2/2.png)

3. Name the Role `EC2ImageBuilderInfraConfigRole` and provide a description.
![role](/images/img_sec7/2/2/3.png)

#### Create pipeline

1. Access the [pipeline console](https://us-east-1.console.aws.amazon.com/imagebuilder/home?region=us-east-1#/createPipeline).

2. Name the pipeline **VSC-Pipeline**. Provide an optional description.
![role](/images/img_sec7/2/3/2.png)
You can also select the option **Enable EC2 image scanning** to ensure the AMI is secure. To do this, AWS Inspector must be enabled.

3. In the Build schedule section, select **Manual**, then click Next.
![role](/images/img_sec7/2/3/3.png)
4. In the Recipe section, select **Create new recipe**. For **Image type**, select **AMI**.
![role](/images/img_sec7/2/3/4.png)
5. In the General section, name the AMI and set its version.

6. In the Base image section, select **Select managed images** and choose **Amazon Linux**.
![role](/images/img_sec7/2/3/6.png)
7. In the Image origin section, select **Quick start**. In the Image name section, select **Amazon Linux 2023 x86**. Choose the compatible version, currently select **Use latest available OS version**.
![role](/images/img_sec7/2/3/7.png)
8. In the Working directory section, enter `/home/ec2-user` as the nvm will be downloaded into the home directory of the executing user **ec2-user**.
![role](/images/img_sec7/2/3/8.png)
9. In the Filter owner section under Components, check the **Owned by me** box. Select the component you created.
![role](/images/img_sec7/2/3/9.png)
In the image, the version is 1.0.1, because a new version was created earlier (the component content remains the same).

In the Test components section for Amazon Linux, for demonstration purposes, select `reboot-test-linux`.
![role](/images/img_sec7/2/3/9b.png)

10. Leave the Storage section as default and click Next.
![role](/images/img_sec7/2/3/10.png)

11. In the Define image creation process section, click Next.

12. In the Infrastructure configuration section, select **Create a new infrastructure configuration**. Name it `VSC-InfraConfig`. Select the IAM role you created.
![role](/images/img_sec7/2/3/12.png)

13. Expand the VPC, subnet, and security groups section. Select the instance type **t3.medium**, VPC **VSC**, choose one of the two private subnets, and select the security group from VSC (with no inbound rules from the previous section) to create and evaluate the instance. Click Next.
![role](/images/img_sec7/2/3/13.png)
A reminder about the security group outbound rules:
![role](/images/img_sec7/2/3/13b.png)

14. Leave the distribution settings as default and click Next.
![role](/images/img_sec7/2/3/14.png)

15. After reviewing, select **Create pipeline**.

