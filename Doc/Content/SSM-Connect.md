<div align="center">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="../Images/SSM-Manager/Light-Architecture.png width=full">
      <source media="(prefers-color-scheme: light)" srcset="../Images/SSM-Manager/Dark-Architecture.png width=full">
      <image alt="Shows a black logo in light color mode and a white one in dark color mode." src="../Images/SSM-Manager/Dark-Architecture.png" width=800></image>
    </picture>
    <h1> 🌟 EC2 Instance SSH Guide via SSM 🌟 </h1>
</div>

![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazon-aws&logoColor=white) [![GitHub](https://img.shields.io/github/license/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9?color=red)](LICENSE) [![Deploy on GitHub Pages](https://img.shields.io/badge/Deploy-GitHub%20Pages-blue)](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/3.ec2_instance_connect/)  

This guide walks you through connecting to an **EC2 Instance in a private subnet** using **AWS Systems Manager (SSM)** and configuring **VSCode** for seamless development.  

[🚀 View the Live Deployment Here](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/8.ssm-connect/)  

---

## 📑 Table of Contents

- [🔧 Prerequisites](#-prerequisites)  
- [🛠️ Step 1: Set Up SSM Endpoints](#️-step-1-set-up-ssm-endpoints)  
- [💼 Step 2: Configure EC2 Role and IAM](#-step-2-configure-ec2-role-and-iam)  
- [🔍 Step 3: Test the Connection](#-step-3-test-the-connection)  
- [📡 Step 4: Use AWS CLI for Connection](#-step-4-use-aws-cli-for-connection)  
- [💻 Step 5: Configure VSCode](#-step-5-configure-vscode)  
- [✨ FAQs](#-faqs)  

---

## 🔧 Prerequisites  

- **VSCode** installed.  
- **AWS CLI v2** installed ([installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)).  
- Access to an EC2 instance and permissions to configure IAM roles.  

---

## 🛠️ Step 1: Set Up SSM Endpoints  

To connect to an EC2 instance in a private subnet:  

1. **Navigate to the VPC Console**:  
   - Go to the **Endpoints** tab.  
   - Click **Create Endpoints**.  

2. **Create Three Endpoints**:  
   - `com.amazonaws.<region>.ssm`  
   - `com.amazonaws.<region>.ssmmessages`  
   - `com.amazonaws.<region>.ec2messages`  

3. **Ensure** the endpoints target the correct VPC and subnet.  
   - Configure the security group to allow communication between the EC2 instance and the endpoints.  

---

## 💼 Step 2: Configure EC2 Role and IAM  

1. **Create an IAM Policy**:  
   Use the JSON below to create a policy:  
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Action": ["ssm:*", "ec2messages:*", "ssmmessages:*"],
               "Resource": "*",
               "Effect": "Allow"
           }
       ]
   }
   ```  

2. **Attach the Policy to a Role**:  
   - Name the role `VscodeOnEc2ForPrototyping`.  
   - Attach the role to the EC2 instance via **Actions → Security → Modify IAM Role**.  

---

## 🔍 Step 3: Test the Connection  

1. Navigate to **EC2 → Instance**.  
2. Look for the **Session Manager** tab.  
3. Click the **Connect** button.  

If unsuccessful:  
- Verify **security group** settings.  
- Ensure the EC2 instance has the **SSM Agent installed**.  
- Align the subnet and endpoint configurations.  

---

## 📡 Step 4: Use AWS CLI for Connection  

### Install AWS CLI  

- **Linux**:  
   ```bash
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
   ```  
- **Windows**:  
   Download from [AWS CLI Installer](https://awscli.amazonaws.com/AWSCLIV2.msi).  

### Test the Connection  

```bash
aws ssm start-session --target <instance_id>
```  

---

## 💻 Step 5: Configure VSCode  

1. Open **SSH Config File**:  
   Add the following:  

   ```ssh
   Host EC2_Instance_SSM
       HostName <your_instance_id>
       User ec2-user
       IdentityFile "path/to/your/key.pem"
       ProxyCommand aws ssm start-session --target %h --document-name AWS-StartSSHSession
   ```  

2. **Connect via Remote-SSH in VSCode**.  

For public instance configuration, [click here](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/8.ssm-connect/).  

---

## ✨ FAQs  

**Q: What if the SSM agent is missing?**  
- Use EC2 Instance Connect to install it.  

**Q: Why can't I see the Connect button?**  
- Ensure IAM role and endpoint settings are correct.  

[📖 Full Documentation Here](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/8.ssm-connect/)  

---

Enjoy seamless SSH connections to your EC2 instances with **SSM and VSCode**! 🎉  
