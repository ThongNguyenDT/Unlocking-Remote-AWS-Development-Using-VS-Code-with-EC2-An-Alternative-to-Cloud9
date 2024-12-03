# Create AMI from instance and run instance from that AMI  🚀

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

