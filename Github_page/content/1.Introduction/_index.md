---
title : "Connect VSCode to AWS EC2 using SSH"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
## 1.1. Introduction to SSH connection

- SSH (**Secure Shell**) is a network protocol used to establish a secure network connection. SSH operates at the upper layer of the TCP/IP layer model. SSH tools (such as OpenSSH) provide users with a way to establish an encrypted network connection to create a private connection channel.

- SSH creates a strong password authentication mechanism, forming an encrypted data communication link between two machines over the internet environment.

- Functions:
- Support remote access to systems and devices using the SSH protocol.

- Allow secure file transfer.

- Execute secure, safe commands on the remote control system.

- Safe and effective management of network infrastructure components.

## 1.2. Purpose of SSH

![Untitled](/images/part1/1-1.2-ssh.jpeg)

The purpose of SSH is to replace the Terminal emulator, the insecure login mechanism (Telnet, Rlogin). The SSH protocol supports the login feature, launching the Terminal Session through the remote control system.

The most basic function of the SSH protocol is to connect to a remote host, corresponding to a Terminal session with the command line "ssh [server.example.org](http://server.example.org/)". This command line can connect the Client to a server [server.example.com](http://server.example.com/) through the user ID UserName.

In case it is the first connection between the Server and the Host, the user must be informed of the Host's key code.

## 1.3. Introducing Remote-SSH Extension on VSCode

![Untitled](/images/part1/1-1.3-extension.png)

**Remote - SSH Extension** allows you to use any remote machine with an SSH server as your development environment. This can significantly simplify development and troubleshooting in a variety of scenarios, allowing you to:

1. Develop on the same operating system you deploy to or use hardware that is larger, faster, or more specialized than your local machine.

2. Quickly switch between different remote development environments and perform secure updates without worrying about affecting your local machine.

3. Access existing development environments from multiple machines or locations.

4. Debug applications running elsewhere, such as a customer site or in the cloud.

5. No need for source code on your local machine to get these benefits because the extension runs commands and other extensions directly on the remote machine. You can open any folder on the remote machine and do 6. things with it just like you would if that folder was on your own machine.

## 1.4. Ubuntu EC2 Initialization Guide

### Step 1: Create VPC

- Navigate to the VPC service page

![Untitled](/images/part1/1-1.4-step1-1.png)

- Select Create VPC

![Untitled](/images/part1/1-1.4-step1-2.png)

![Untitled](/images/part1/1-1.4-step1-3.png)
- Select: Create VPC, wait a moment and we will get the result like this. Next, we can click View VPC to see

![Untitled](/images/part1/1-1.4-step1-4.png)

![Untitled](/images/part1/1-1.4-step1-5.png)

### Step 2: Navigate to the EC2 service page.

- You can type the keyword “EC2” in the search bar to navigate to the service page.
- Here, you select Launch instance **to initialize EC2 instance.**

![Untitled](/images/part1/1-1.4-step2-1.png)

### Step 3: Initialize EC2

- **Set up information about Name and Application and OS Images:**

- Name: **remote-connection**
- **Application and OS Images**: Ubuntu

![Untitled](/images/part1/1-1.4-step3-1.png)

- **Set up information about AMI:**

- AMI: Ubuntu Server 24.04 LTS (HVM), SSD Volume Type (default)

![Untitled](/images/part1/1-1.4-step3-2.png)

- Select Instance Type:
- Instance Type: t2.micro

![Untitled](/images/part1/1-1.4-step3-3.png)

- Create key pair:

- Select: Create new key pair

- Name: remote_connect

- Type: RSA

- Private key file: .pem
- Select Create new key pair to create key pair

![Untitled](/images/part1/1-1.4-step3-4.png)

![Untitled](/images/part1/1-1.4-step3-5.png)

- We will receive 1 key pair:

![Untitled](/images/part1/1-1.4-step3-6.png)

- In the Networking settings section, we choose according to the form below. At Firewall, we select: security group

![Untitled](/images/part1/1-1.4-step3-7.png)

- Finally, select, Launch instance to initialize Ec2 that has just been set up. The result is

![Untitled](/images/part1/1-1.4-step3-8.png)

## 1.5. Instructions for configuring Command Prompt connection with EC2

### Step 1: Open Command Prompt

- `Win + R` and type “cmd” to open Command Prompt

![Untitled](/images/part1/1-1.5-step1-1.png)

- The Command Prompt panel is opened, you use the command `cd Downloads` to navigate to the folder containing the file `testing-remote-connection.pem`

![Untitled](/images/part1/1-1.5-step1-2.png)

### Step 2: Connect SSH

- In the EC2 service workspace, select the EC2 you want to connect to SSH. Next, select `Connect`

![Untitled](/images/part1/1-1.5-step2-1.png)

- Copy the command `ssh -i "testing-remote-connection.pem" [ubuntu@ec2-13-212-36-87.ap-southeast-1.compute.amazonaws.](mailto:ubuntu@ec2-13-212-36-87.ap-southeast-1.compute.amazonaws.com)com`

![Untitled](/images/part1/1-1.5-step2-2.png)

- Paste the copied command into the Command Prompt. Next, type “yes” to confirm the connection.

![Untitled](/images/part1/1-1.5-step2-3.png)

- Connection successful

![Untitled](/images/part1/1-1.5-step2-4.png)
- You can check by creating a [test.py](http://test.py) file in the “test” folder using the command lines as shown below.

![Untitled](/images/part1/1-1.5-step2-5.png)

- As a result, the victim will receive the working screen of the [test.py](http://test.py) file

![Untitled](/images/part1/1-1.5-step2-6.png)

## 1.6. Instructions for configuring VSCode connection with EC2

- Install Remote-SSH utility to support connecting to SSH Host

![Untitled](/images/part1/1-1.6-1.png)

- After installing the utility, you will see the icon at the bottom left of the screen.

![Untitled](/images/part1/1-1.6-2.png)

- Click on the icon to open the connection selection panel. Select “Connect to Host…”

![Untitled](/images/part1/1-1.6-3.png)

- Select “Add New SSH Host” to create a new SSH Host

![Untitled](/images/part1/1-1.6-4.png)

- Select “C:\Users\Legion\.ssh\config” to open the config file

![Untitled](/images/part1/1-1.6-5.png)
- In the EC2 workspace, select “Details” to get the necessary information for setting up SSH Host

```
Host remote-connection
HostName 13.212.36.87
User ubuntu
IdentityFile "C:\Users\Legion\Downloads\testing-remote-connection.pem"
```
- After setting up, click on the icon to open you select and navigate to “remote-connection”

![Untitled](/images/part1/1-1.6-6.png)

- Next, you select Platform details “Linux” and select “Continue”

![Untitled](/images/part1/1-1.6-7.png)

- Success when your friend works on “SSH Host: remote-connection” open

![Untitled](/images/part1/1-1.6-8.png)

- Next you can navigate to your test folder by selecting File, selecting Open folder and navigating to the test folder.
![Untitled](/images/part1/1-1.6-9.png)
