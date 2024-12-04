<div align="center">
    <h1> 🚀 Code Server Setup with CloudFormation  </h1>
</div>
![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazon-aws&logoColor=white) [![GitHub](https://img.shields.io/github/license/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9?color=red)](LICENSE) [![Deploy on GitHub Pages](https://img.shields.io/badge/Deploy-GitHub%20Pages-blue)](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/10.cloudfrontdistribution/)  

A comprehensive guide to deploying VS Code Server on AWS using CloudFormation with CloudFront distribution for secure access.

📑 Table of Contents
Create VPC Infrastructure
Deploy CloudFormation Stack
Configure AWS CLI & Tools
Access VS Code Server

1️⃣ Create VPC Infrastructure
🌐 Requirements:
Private subnet for EC2 instance (min 1)
NAT Gateway for internet access
Public subnet for Internet Gateway
🛠 Steps:
Navigate to VPC Dashboard
Select "VPC and more"
Configure settings:
Name your VPC
Select CIDR range
Choose AZ configuration

2️⃣ Deploy CloudFormation Stack
📋 Region Support:
CloudFrontPrefixListIdMappings:
  us-west-2:
    PrefixListId: "pl-82a045eb"
  us-east-1:
    PrefixListId: "pl-3b927c52"
  ap-southeast-1:
    PrefixListId: "pl-31a34658"
🚀 Deployment Steps:
Create new CloudFormation stack
Upload template
Configure parameters:
Stack name: code-server-stack
VPC ID: Select created VPC
Subnet ID: Choose private subnet

3️⃣ Configure AWS CLI & Tools
⚙️ AWS CLI Setup:
aws configure
# Enter:
# - AWS Access Key ID
# - AWS Secret Access Key
# - Default region
# - Output format
🔧 Session Manager Plugin:
# Windows: Download from AWS website
# MacOS:
brew install --cask session-manager-plugin
# Linux:
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
sudo dpkg -i session-manager-plugin.deb

4️⃣ Access VS Code Server
🔄 Port Forwarding:
aws ssm start-session \
  --target <instance-ID> \
  --document-name AWS-StartPortForwardingSessionToRemoteHost \
  --parameters host="<Private-IP>",portNumber:"8080",localPortNumber:"8080"
🌐 Verification:
curl localhost:8080
📝 License
This project is licensed under the MIT License. See the LICENSE file for details.

🎨 Built to provide a seamless cloud IDE experience with enterprise-grade security!