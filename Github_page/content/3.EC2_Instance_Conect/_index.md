---
title: "Connecting VSCode to a Private EC2 via Instance Connect Endpoint"  
date: "`r Sys.Date()`"  
weight: 3  
chapter: False  
pre: " <b> 3. </b> "
---

In this section, we will establish a remote SSH connection from VSCode to an EC2 instance located in a private subnet.

---

### Step 1: Set Up Necessary Endpoints  

To SSH into an EC2 instance within a private subnet, you need to connect via an intermediary device, such as a **_bastion host_** or **_endpoint_**.  

Here, we'll set up the connection using an **EC2 Instance Connect Endpoint**.  

This step is required only if you want to connect to an **Instance** located in a **private subnet**:  
- Search for the ***`VPC`*** service.  
- Select **VPC**.  
- Go to the **Endpoints** tab.  
- Click **Create Endpoint**.  

![Untitled](/images/EC2_Instance_Connect_Endoint/image_1.png)  

Next:  
- Assign a name.  
- Choose **EC2 Instance Connect Endpoint**.  
- Select the appropriate VPC.  
- Optionally restrict connecting IPs under **Additional settings** -> check **Preserve Client IP**.  
- Choose a Security Group for the endpoint; if left blank, AWS uses the default Security Group.  

    Minimum requirements:  
    - **Outbound rule**: Allow SSH to the EC2 instance.  
    - **Inbound rule**: Allow SSH connections from the endpoint.  
    - Refer to the Security Group configurations for **EC2 Instance Connect Endpoint** at [📦AWS | Doc | Security groups for EC2 Instance Connect Endpoint](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/eice-security-groups.html).  
- Select a subnet that includes your EC2 instance.  
- Click **Create Endpoint**.  

![Untitled](/images/EC2_Instance_Connect_Endoint/image_2.png)  

After the endpoint creation process completes:  

![image.png](/images/EC2_Instance_Connect_Endoint/image_3.png)  

{{% notice tip %}}
♦️ **EC2 Instance Connect** allows us to directly create a VPC **endpoint** in the EC2 **Connect** section without needing to access the **VPC** service.  
♦️ **EC2 Instance Connect** does not require additional plugins to be set up.  
♦️ **EC2 Instance Connect** does not require access permissions to connect EC2 to the management service, as it establishes a direct connection without involving a management service.  
♦️ It can only connect to one **EC2** instance at a time, requiring configuration for each machine individually.  
♦️ Configuration is simpler compared to **SSM**.  
{{% /notice %}}

---

### Step 2: Test the Connection  

Go back to the **EC2** service -> **Instances** section -> in your instance connect tab.  

![image.png](/images/EC2_Instance_Connect_Endoint/image_4.png)  

If you see the three warnings and cannot connect to your EC2 via the EC2 Instance Connect tab, it indicates that your VPC and subnet settings are correct.  

Choose **Connect with EC2 Instance Connect Endpoint**.  

![image.png](/images/EC2_Instance_Connect_Endoint/image_5.png)  

If the connection button turns yellow, your Security Group and endpoint settings are correctly configured:  

![image.png](/images/EC2_Instance_Connect_Endoint/image_6.png)  

- Click **Connect**, and you should now be able to connect to your EC2 instance through the endpoint.  

![image.png](/images/EC2_Instance_Connect_Endoint/image_7.png)  

If the connection fails, check the following:  
- Ensure the Security Group settings are correct. If unsure, use the default Security Group for both the endpoint and EC2 instance.  
- Verify that the endpoint and your instances are configured within the same private subnet. Note that the subnet selection for the endpoint in Step 1 targets where the endpoint connects rather than where it is located.  
- No endpoint configuration is required for public subnets.  

---

### Step 3: Connect Using CLI  

The next steps require AWS CLI v2 installed on your machine.  

If not already installed, follow these instructions:  

Refer to [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).  

- For _Linux_:  
    ```bash  
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"  
    unzip awscliv2.zip  
    sudo ./aws/install  
    ```  

- For _Windows_:  
    - Download and install AWS CLI from [`https://awscli.amazonaws.com/AWSCLIV2.msi`](https://awscli.amazonaws.com/AWSCLIV2.msi).  

- Verify the CLI installation with the command:  
    ```bash  
    aws --version  
    ```  

To connect via CLI, run the following command:  

    aws ec2-instance-connect ssh --instance-id <instance_id>   

If the result appears as shown below, your connection was successful:  
![image.png](/images/EC2_Instance_Connect_Endoint/image_8.png)  

---

### Step 4: Configure VSCode  

Now that you’ve successfully connected to the EC2 instance via CLI, the next step is to configure VSCode for this connection.  

[Follow the same steps as connecting to an EC2 in a public subnet](/2).  

In the host editing step, create SSH tunnels instead of using a public IP.  

The configuration is straightforward. Update your host file as follows:  

```powershell  
Host EC2_Instance_Connect_Endpoint  
    HostName <your_instance_id>  
    User ec2-user  
    IdentityFile "your/path/to/your/key.pem"  
    ProxyCommand aws ec2-instance-connect open-tunnel --instance-id %h
```  

---