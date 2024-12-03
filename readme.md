# Host VSCode from AMI from EC2 Image Builder 🚀

## 🌟 Overview

EC2 Image Builder automates the creation, management, and deployment of customized, secure, and up-to-date “golden” AMIs that are pre-installed and pre-configured with software and settings.

In this section, we will build an AMI similar to last section, but using EC2 Image Builder to create an image pipeline from infrastructure configuration (OS, VPC, subnet, security group), IAM instance profile, to installing the AMI, deploying it to an instance, testing the state of that instance, and scanning for security vulnerabilities in the ami.

## ✨ Key Features

- Pre-configured VS Code Server environment 
- Image pipeline
- Easy to develop and maintain

## 🏗️ Architecture

The solution deploys:
- Image pipeline with EC2 Image Builder
- EC2 instance running Code Server from the image created by Image Builder
- Security groups and IAM roles 
- VPC networking components (optional)
- SSM Session Manager port forwarding
