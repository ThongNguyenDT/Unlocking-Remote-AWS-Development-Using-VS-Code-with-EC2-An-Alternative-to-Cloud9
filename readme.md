<div align="center">
    <image src="Github_page/static/images/thumb.png" width=full></image>
    <h1> <span>Unlocking Remote AWS Development</span> <br>
    <span>Using VS Code with EC2:</span> <br>
    <span>An Alternative to Cloud9</span></h1>
</div>


![AWS](https://img.shields.io/badge/AWS-orange?logo=amazon-aws&logoColor=white)
![AWS EC2](https://img.shields.io/badge/AWS%20EC2-blue?logo=amazon-aws&logoColor=white)
![AWS VPC](https://img.shields.io/badge/AWS%20VPC-green?logo=amazon-aws&logoColor=white)
![AWS VPC Endpoint](https://img.shields.io/badge/AWS%20VPC%20Endpoint-red?logo=amazon-aws&logoColor=white)
![AWS AMI](https://img.shields.io/badge/AWS%20AMI-purple?logo=amazon-aws&logoColor=white)
![AWS CloudFront](https://img.shields.io/badge/AWS%20CloudFront-teal?logo=amazon-aws&logoColor=white)
![VSCode](https://img.shields.io/badge/VSCode-blue?logo=visual-studio-code&logoColor=white)
![AWS IAM](https://img.shields.io/badge/AWS%20IAM-yellow?logo=amazon-aws&logoColor=white)
![CloudFormation](https://img.shields.io/badge/CloudFormation-brown?logo=amazon-aws&logoColor=white)




> **💻 Seamlessly integrate Amazon EC2 with Visual Studio Code for an unparalleled remote development experience!**  

Welcome to the **"Unlocking Remote AWS Development Using VS Code with EC2"** project! This repository serves as a comprehensive guide to using Visual Studio Code with Amazon EC2. You'll explore methods for connecting VS Code to EC2 instances, hosting VS Code as a web application, and building automated, secure environments for modern development.  

---

## 📚 **Table of Contents**
1. **Introduction**  
2. **Remote SSH Extension**  
3. **Connecting VS Code to a Private EC2 via Instance Connect Endpoint**  
4. **Hosting VS Code on EC2 from Amazon Linux 2**  
5. **Manually Deploy VS Code on EC2**  
6. **Create AMI from Instance and Run Instances from That AMI**  
7. **Host VS Code from AMI Built with EC2 Image Builder**  
8. **Connecting VS Code to a Private EC2 via AWS System Manager**  
9. **Hosting Code Server on EC2 with CloudFront Distribution**

---

## 📘 **Documentation**  

Each tutorial in this guide is stored in the [`Content`](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content) folder of this repository. Below is an overview of each topic:  

### 1. **Introduction**  
**🔗 [Introduction.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9)**  
Start your journey with an overview of the prerequisites, key tools, and foundational knowledge for working with Visual Studio Code and Amazon EC2.

---

### 2. **Remote SSH Extension**  
**🔗 [SSH-Connect.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/SSH-Connect.md)**  
Discover how to use the VS Code *Remote SSH* extension to securely connect and manage Amazon EC2 instances.  

---

### 3. **Connecting VS Code to a Private EC2 via Instance Connect Endpoint**  
**🔗 [EC2-Instance-Connect.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/EC2-Instance-Connect.md)**  
Learn how to securely access private EC2 instances through AWS Instance Connect Endpoint.  

---

### 4. **Hosting VS Code on EC2 from Amazon Linux 2**  
**🔗 [Hosting-VS-Code-on-EC2-from-Amazon-Linux-2.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/Hosting-VS-Code-on-EC2-from-Amazon-Linux-2.md)**  
Host a fully functional web-based version of VS Code on an EC2 instance running Amazon Linux 2.  

---

### 5. **Manually Deploy VS Code on EC2**  
**🔗 [Manually-deploy-VS-Code-on-EC2.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/Manually-deploy-VS-Code-on-EC2.md)**  
Get hands-on experience configuring and deploying VS Code or Code Server on an EC2 instance.  

---

### 6. **Create AMI from Instance and Run Instances from That AMI**  
**🔗 [Create-AMI-from-instance-and-run-instance-from-that-AMI.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/Create-AMI-from-instance-and-run-instance-from-that-AMI.md)**  
Learn to create a reusable Amazon Machine Image (AMI) to save and replicate your configured environment.  

---

### 7. **Host VS Code from AMI Built with EC2 Image Builder**  
**🔗 [Host-VSCode-from-AMI-from-EC2-Image-Builder.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/Host-VSCode-from-AMI-from-EC2-Image-Builder.md)**  
Automate the creation of AMIs with pre-installed VS Code using EC2 Image Builder.  

---

### 8. **Connecting VS Code to a Private EC2 via AWS System Manager**  
**🔗 [SSM-Connect.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/SSM-Connect.md)**  
Use AWS Systems Manager to connect securely to private EC2 instances without requiring a public IP address.  

---

### 9. **Hosting Code Server on EC2 with CloudFront Distribution**  
**🔗 [CloudFrontDistribution.md](https://github.com/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/tree/develop/Doc/Content/CloudFrontDistribution.md)**  
Host Code Server on EC2 and distribute it securely with Amazon CloudFront for optimized global access.  

---

## 🌟 **Contributing**  

We welcome contributions to enhance this project! Whether it's improving documentation, adding examples, or suggesting new use cases, your input is highly valued.  

1. Fork the repository.  
2. Create a feature branch.  
3. Submit a pull request.  

---


## 📧 **Feedback**  
For questions, suggestions, or support, please open an issue in this repository or contact the maintainer.  

> Let’s unlock the full potential of remote development with AWS and VS Code! 🚀