
<div align="center">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="../Images/EC2-Instance-Connect/Light-Architecture.png width=full">
      <source media="(prefers-color-scheme: light)" srcset="../Images/EC2-Instance-Connect/Dark-Architecture.png width=full">
      <image alt="Shows a black logo in light color mode and a white one in dark color mode." src="../Images/EC2-Instance-Connect/Dark-Architecture.png" width=500></image>
    </picture>
    <h1> 🚀 EC2 Instance Connect via VSCode & CLI  </h1>
</div>

![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazon-aws&logoColor=white) [![GitHub](https://img.shields.io/github/license/your-repo/license)](LICENSE) [![Deploy on GitHub Pages](https://img.shields.io/badge/Deploy-GitHub%20Pages-blue)](https://your-repo.github.io)  

A step-by-step guide to setting up an **SSH connection** from VSCode to an EC2 instance in a **private subnet**, using **EC2 Instance Connect Endpoint**.

---

## 📑 Table of Contents  
1. [Set Up EC2 Instance Connect Endpoint](#1-set-up-ec2-instance-connect-endpoint)  
2. [Test the Connection](#2-test-the-connection)  
3. [Connect Using AWS CLI](#3-connect-using-aws-cli)  
4. [Configure VSCode for SSH](#4-configure-vscode-for-ssh)  

---

## 1️⃣ Set Up EC2 Instance Connect Endpoint  

To connect to an **EC2 instance in a private subnet**, an **EC2 Instance Connect Endpoint** is required.  

### 🛠 Steps:  
1. **Navigate to VPC Service**  
   - Go to the AWS **VPC** service.  
   - Select the **Endpoints** tab.  
   - Click **Create Endpoint**.  

2. **Configure the Endpoint**  
   - Assign a **name** (e.g., *MyConnectEndpoint*).  
   - Choose **EC2 Instance Connect Endpoint** as the service type.  
   - Select the appropriate **VPC** and **subnet** where your EC2 instance resides.  
   - Configure **Security Groups**:  
     - **Inbound rule**: Allow SSH from the endpoint.  
     - **Outbound rule**: Allow SSH to the EC2 instance.  

   For detailed security configurations, refer to [AWS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/eice-security-groups.html).  

3. **Review & Create**  
   - Click **Create Endpoint**.  

🎉 **Tip**: Use the EC2 **Connect** section for simpler configuration without needing the VPC service.

---

## 2️⃣ Test the Connection  

### 🔍 Steps:  
1. Go to **EC2 Service** → **Instances**.  
2. Select your EC2 instance.  
3. Under the **Connect** tab:  
   - Choose **EC2 Instance Connect Endpoint**.  
   - Click **Connect**.  

🎯 If the button turns yellow, your endpoint and security group settings are correctly configured.  

### ❌ Troubleshooting:  
- Ensure the Security Group allows SSH traffic.  
- Verify that the endpoint and instance are in the same private subnet.  
- Public subnets do not require endpoint configuration.

---

## 3️⃣ Connect Using AWS CLI  

### 🔧 Install AWS CLI (If Not Installed):  
- **Linux**:  
  ```bash
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"  
  unzip awscliv2.zip  
  sudo ./aws/install  
  ```  

- **Windows**:  
  [Download AWS CLI](https://awscli.amazonaws.com/AWSCLIV2.msi) and install it.  

- **Verify Installation**:  
  ```bash
  aws --version  
  ```  

### ⚡ Connect to EC2 via CLI:  
Run the following command:  
```bash
aws ec2-instance-connect ssh --instance-id <instance_id>
```  

If successful, you'll see the terminal connected to your EC2 instance.

---

## 4️⃣ Configure VSCode for SSH  

### 💻 Update SSH Configuration:  
Add the following to your `~/.ssh/config` file:  
```plaintext
Host EC2_Instance_Connect_Endpoint  
    HostName <your_instance_id>  
    User ec2-user  
    IdentityFile "your/path/to/your/key.pem"  
    ProxyCommand aws ec2-instance-connect open-tunnel --instance-id %h
```  

### 🛠 Open VSCode:  
1. Install the **Remote - SSH** extension.  
2. Use the **Remote Explorer** panel to connect to your EC2 instance via the configured SSH tunnel.

---

### 🌐 [Detailed Deploy Guide on GitHub Pages](https://your-repo.github.io)  

---

## 📝 License  
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.  

---

🎨 Designed to make your **EC2 instance connection** simple, efficient, and visually engaging!