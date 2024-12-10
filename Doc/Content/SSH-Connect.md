<div align="center">
    <h1> 🚀 **Remote Connection Setup Guide with SSH & EC2** 🌐 </h1>
</div>

![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazon-aws&logoColor=white) [![GitHub](https://img.shields.io/github/license/ThongNguyenDT/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9?color=red)](LICENSE) [![Deploy on GitHub Pages](https://img.shields.io/badge/Deploy-GitHub%20Pages-blue)](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9)

Welcome to the **Remote Connection Setup Guide**! This repository provides a step-by-step walkthrough to configure and connect to remote environments using **SSH** and **AWS EC2**, ensuring secure, efficient, and hassle-free remote development.

🌍 **Live Demo**: Check out the deployed guide [here](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/2.remote-ssh/).

---

## 📜 **Table of Contents**
1. [🔑 Introduction to SSH](#-introduction-to-ssh)
2. [🎯 Purpose of SSH](#-purpose-of-ssh)
3. [💻 Remote-SSH Extension in VSCode](#-remote-ssh-extension-in-vscode)
4. [🌐 Ubuntu EC2 Initialization Guide](#-ubuntu-ec2-initialization-guide)
   - [Step 1: Create VPC](#step-1-create-vpc)
   - [Step 2: Navigate to EC2 Service](#step-2-navigate-to-ec2-service)
   - [Step 3: Initialize EC2](#step-3-initialize-ec2)
5. [📂 Configuring Command Prompt with EC2](#-configuring-command-prompt-with-ec2)
6. [🖥️ Configuring VSCode with EC2](#-configuring-vscode-with-ec2)

---

## 🔑 **Introduction to SSH**

- **SSH (Secure Shell)** is a protocol used to establish a secure and encrypted connection between systems.
- **Key Features**:
  - Secure remote access to devices and systems.
  - File transfer with encryption.
  - Execute commands remotely.
  - Manage network infrastructure securely.

---

## 🎯 **Purpose of SSH**
SSH replaces insecure protocols like Telnet and Rlogin by providing a secure channel for:
- Remote system login and terminal session management.
- Connection via commands such as:
  ```bash
  ssh -i <private_key> username@hostname
  ```

---

## 💻 **Remote-SSH Extension in VSCode**
The **Remote-SSH Extension** allows you to:
- Develop directly on remote systems with SSH.
- Debug apps running on remote hosts or cloud environments.
- Eliminate local dependencies while working remotely.

---

## 🌐 **Ubuntu EC2 Initialization Guide**

### Step 1: Create VPC
1. Go to the **VPC Service** page.
2. Select **Create VPC** and follow the prompts.
3. After creation, click **View VPC** to verify.


---

### Step 2: Navigate to EC2 Service
- Search for **EC2** in the AWS search bar.
- Click **Launch Instance** to begin EC2 setup.


---

### Step 3: Initialize EC2
1. **Set Instance Details**:
   - Name: `remote-connection`
   - OS: **Ubuntu Server 24.04 LTS**
   - Instance Type: `t2.micro`

2. **Create Key Pair**:
   - Name: `remote_connect`
   - Type: RSA, format: `.pem`

3. **Networking**:
   - Configure Security Group.

4. **Launch Instance**:
   - Click **Launch** and verify.

---

## 📂 **Configuring Command Prompt with EC2**

### Step 1: Open Command Prompt
1. Press `Win + R`, type `cmd`, and navigate to the folder containing your `.pem` file:
   ```bash
   cd Downloads
   ```

2. Confirm the `.pem` file exists in the folder.


---

### Step 2: Connect to EC2
1. In AWS EC2 Dashboard, copy the **SSH Command**:
   ```bash
   ssh -i "your-key.pem" ubuntu@<public-ip>
   ```

2. Paste it into the terminal and type `yes` to confirm.

---

## 🖥️ **Configuring VSCode with EC2**

### Step 1: Install Remote-SSH Extension
1. Install the **Remote-SSH** extension in VSCode.

2. Click the **Remote-SSH Icon** in the bottom-left corner.


---

### Step 2: Add SSH Host
1. Select **Add New SSH Host**.
2. Configure your SSH settings in `config` file:
   ```plaintext
   Host remote-connection
   HostName <EC2-Public-IP>
   User ubuntu
   IdentityFile <path-to-key.pem>
   ```

3. Click **Connect to Host** and choose `remote-connection`.


---

## 🌟 **Ready to Explore?**
Head over to the **[Deployed Guide](https://thongnguyendt.github.io/Unlocking-Remote-AWS-Development-Using-VS-Code-with-EC2-An-Alternative-to-Cloud9/2.remote-ssh/)** for an interactive experience! 🎉
