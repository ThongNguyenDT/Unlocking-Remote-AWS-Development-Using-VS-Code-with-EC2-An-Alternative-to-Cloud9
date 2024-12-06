<div align="center">
    <h1> Create AMI from instance and run instance from that AMI  🚀 </h1>
</div>

![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazon-aws&logoColor=white) [![GitHub](https://img.shields.io/github/license/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9?color=red)](LICENSE) [![Deploy on GitHub Pages](https://img.shields.io/badge/Deploy-GitHub%20Pages-blue)](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9)

🌍 **Live Tutorial**: Check out the deployed guide [here](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/6.create-ami-from-instance-and-run-instance-from-that-ami/).


## 🌟 Overview

This project provides a solution to build an AMI that has already installed VS Code Server (instead of installing VS Code during launching EC2), which makes the EC2 deployment on AWS faster. 

## ✨ Key Features

- Pre-configured VS Code Server environment 
- Faster EC2 launching
- Access from anywhere via HTTPS

## 🏗️ Architecture

The solution deploys:
- AMI that has already installed VS Code Server
- EC2 instance running Code Server
- Security groups and IAM roles 
- VPC networking components (optional)
- SSM Session Manager port forwarding