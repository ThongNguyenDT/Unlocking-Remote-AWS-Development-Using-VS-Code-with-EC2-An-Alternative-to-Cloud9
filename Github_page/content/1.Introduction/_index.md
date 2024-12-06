---
title: "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Welcome to this hands-on tutorial workshop, designed to take your development workflow to the next level by connecting VSCode with Amazon EC2 instances. Whether you’re accessing private instances, running VSCode in the cloud, or hosting a browser-based VSCode environment, this workshop will guide you through practical, real-world solutions to modern challenges in cloud-based development.  

### What You’ll Learn  
In this workshop, we’ll explore a series of methods to connect and host VSCode in AWS environments. These techniques are applicable whether your instance has a public IP or is entirely private. By the end, you'll have gained a comprehensive toolkit to elevate your development capabilities using a combination of EC2, VSCode, and AWS services.  

### How This Workshop is Structured  
The content is framed as a set of challenges, with each challenge representing a new method of integrating VSCode with EC2. You’ll learn to:  
1. **Remote Development with VS Code:** Explore the powerful *Remote Development* extensions for connecting VS Code to EC2 instances.  
2. **Handling Private EC2 Instances:** Learn to work with private EC2 instances using AWS services like Instance Connect Endpoint and AWS Systems Manager.  
3. **Hosting VS Code as a Web App:** Set up VS Code or its open-source alternative (Code Server) as a web-based development environment.  
4. **Building and Deploying Custom AMIs:** Create Amazon Machine Images (AMIs) with pre-configured VS Code setups for fast and consistent deployments.  
5. **Securing Remote Access:** Discover how to secure remote access using CloudFront, secure tunneling, and more.

This approach allows you to learn step-by-step, adapting solutions to fit your specific project needs and infrastructure.  

### Challenges Overview  

1. **[Introduction](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/1.introduction/)**: Understand the prerequisites and set up your environment for working with EC2 and VS Code.

2. **[Remote SSH Extension](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/2.remote-ssh/)**: Learn how to use VS Code's *Remote SSH* extension to connect directly to EC2 instances.

3. **[Connecting VS Code to a Private EC2 via Instance Connect Endpoint](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/3.ec2_instance_connect/)**: Configure and use AWS Instance Connect Endpoint to establish secure connections to private EC2 instances.

4. **[Hosting VS Code on Amazon Linux 2](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/4.hosting-vs-code-on-ec2-from-amazon-linux-2/)**: Host a web-accessible version of VS Code on an Amazon Linux 2 EC2 instance.

5. **[Manual Deployment of VS Code on EC2](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/5.manually-deploy-vs-code-on-ec2/)**: Manually install and configure VS Code or Code Server on an EC2 instance for development.

6. **[Create an AMI from an Instance](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/6.create-ami-from-instance-and-run-instance-from-that-ami/)**: Learn how to save your configuration by creating an AMI from a configured instance and deploying new instances from that AMI.

7. **[Host VS Code with EC2 Image Builder](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/7.host-vscode-from-ami-from-ec2-image-builder/)**: Use EC2 Image Builder to automate the creation of AMIs pre-configured with VS Code or Code Server.

8. **[Connecting VS Code to a Private EC2 via AWS Systems Manager](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/8.ssm-connect/)**: Leverage AWS Systems Manager to connect to private EC2 instances securely, without requiring a public IP address.

9. **[Hosting Code Server on EC2 with CloudFront Distribution](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/10.cloudfrontdistribution/)**: Set up Code Server on EC2 and distribute it securely via Amazon CloudFront.


### Who Should Attend?  
This workshop is perfect for:  
- Developers and DevOps engineers working with AWS who want to streamline their cloud development environments.  
- Individuals looking to deepen their understanding of AWS services and advanced networking configurations.  
- Teams aiming to simplify collaboration with remote VSCode setups.  

No matter your background, you'll find value in learning to customize and automate remote development environments. Let’s get started! 🚀
