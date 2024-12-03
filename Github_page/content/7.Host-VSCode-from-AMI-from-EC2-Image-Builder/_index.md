---
title: "Host VSCode from AMI built with EC2 Image Builder"  
date: "`r Sys.Date()`"  
weight: 7 
chapter: false  
pre: " <b> 7. </b> "  
---

EC2 Image Builder helps automate the creation, management, and deployment of customized, secure, and up-to-date "golden" AMIs, pre-installed and pre-configured with software and settings.

In this section, we will build an AMI like in part 5, but by using EC2 Image Builder to create an image pipeline process, starting from infrastructure configuration (OS, VPC, subnet, security group), IAM instance profile, to installing the AMI, deploying it as an instance, testing the state of the instance, and scanning for security vulnerabilities within the AMI.

Reference: [AWS for Microsoft Workloads Immersion Day](https://catalog.us-east-1.prod.workshops.aws/workshops/d6c7ecdc-c75f-4ad1-910f-fdd994cc4aed/en-US)

#### Terminology

##### AMI  
The basic deployment unit in Amazon EC2. An AMI is a pre-configured VM image containing the operating system and pre-installed software to deploy EC2 instances.

##### Image Pipeline  
An automated configuration to build secure operating system images on AWS. An Image Builder pipeline is associated with an image recipe that defines the stages of build, validation, and test for the image lifecycle. The image pipeline can be linked with an infrastructure configuration that defines where the image is built. You can define attributes such as version, subnet, security groups, logging, and other infrastructure-related configurations.

##### Source Image  
An image recipe is a document that defines the source image and components that will be applied to the source image to create the desired output image configuration. You can use an image recipe to replicate builds. Image Builder recipes can be shared, branched, and edited using the console, AWS CLI, or API.

##### Build Components  
Orchestration documents define the sequence of steps to download, install, and configure software packages. They also define validation steps and security hardening. A component is defined using the YAML document format.
