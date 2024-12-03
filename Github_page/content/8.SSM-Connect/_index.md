---
title: "Connecting VSCode to a Private EC2 via AWS System Manager"  
date: "`r Sys.Date()`"  
weight: 8.
chapter: False  
pre: " <b> 8. </b> "
---

In this section, we will establish a remote SSH connection from VSCode to an EC2 instance located in a private subnet.

---

### Step 1: Setup Required Endpoints

To SSH into an EC2 instance in a private subnet, you must connect through an intermediary, which can be a **_bastion host_** or **_endpoints_**.

In this guide, we will use the **SSM Endpoint**.

This step is necessary only if you want to connect to an **instance** located in a **private subnet**:
- Search for the ***`VPC`*** service.
- Select **VPC**.
- Go to the **Endpoints** tab.
- Click **Create endpoints**.

![Untitled](/images/SSM-Connect/image_1.png)

Next:
- Create the three SSM endpoint connections as follows:
    - Assign a name.
    - Search for ***ssm***.
    - Select options as shown below.
    ![Untitled](/images/SSM-Connect/image_2.png)
  - As in the steps for creating an endpoint in [EC2_Instance_Connect](/vi/3.ec2_instance_connect/), you need to create three endpoints:
    - `com.amazonaws.us-east-1.ssm`
    - `com.amazonaws.us-east-1.ssmmessages`
    - `com.amazonaws.us-east-1.ec2messages`

Once the endpoint creation process is complete:

![image.png](/images/SSM-Connect/image_3.png)
![image.png](/images/SSM-Connect/image_4.png)
![image.png](/images/SSM-Connect/image_5.png)
{{% notice tip %}}
♦️ Ensure the endpoint targets the VPC and subnet of the EC2 instance you wish to connect to.  
♦️ Open the security group to allow the EC2 instance and the endpoint to communicate.  
♦️ Create the three endpoints mentioned above:  
... ♦️ `com.amazonaws.us-east-1.ssm`  
... ♦️ `com.amazonaws.us-east-1.ssmmessages`  
... ♦️ `com.amazonaws.us-east-1.ec2messages`  
{{% /notice %}}
{{% notice tip %}}
♦️ SSM is a management service, so you **must grant permissions** for the EC2 instance to interact with it. Ensure your instance has the AWS Systems Manager agent installed.  
... ♦️ Some **AMI** images come pre-configured with the agent.  
... ♦️ [Learn more here](https://docs.aws.amazon.com/systems-manager/latest/userguide/ami-preinstalled-agent.html).  
♦️ This allows you to configure multiple devices simultaneously using the [**AWS Systems Manager**](https://us-east-1.console.aws.amazon.com/systems-manager/home) service.  
{{% /notice %}}

---

### Step 2: Configure the EC2 Instance

To connect using the session manager, you need to assign a role to the EC2 instance.

#### Create a Role Manually

##### Create a Policy

Create a JSON policy, and name it, for example, `VSCodeBastionHostInstanceRoleDefaultPolicy`.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "*",
                "ec2messages:*",
                "ssm:UpdateInstanceInformation",
                "ssmmessages:*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

Result:

![image.png](/images/SSM-Connect/image_6.png)

##### Create a Role and Attach the Policy

Name the role `VscodeOnEc2ForPrototyping`.

![image.png](/images/SSM-Connect/image_7.png)

The role is used for the IAM instance profile.

#### Use an Existing Role

- Search for **IAM** in the search bar.
- Select the **IAM** service.
- Go to the **Role** tab.
- Click **Create Role**.

![image.png](/images/SSM-Connect/image_8.png)

- Select **AWS Service**.
- Choose **EC2**.
- Select **EC2 Role for AWS Systems Manager**.
- Click **Next**.

![image.png](/images/SSM-Connect/image_9.png)

- Proceed by clicking **Next**.

![image.png](/images/SSM-Connect/image_10.png)

- Name the role and click **Create Role**.

![image.png](/images/SSM-Connect/image_11.png)

#### Attach the Role

- Return to the **Instances** tab.
- Select your **Instance**.
- Click **Action → Security → Modify IAM role**.

![image.png](/images/SSM-Connect/image_12.png)

- Select the role and update the IAM role.

![image.png](/images/SSM-Connect/image_13.png)

---

### Step 3: Test the Connection

Return to the **EC2** -> **Instance** interface.

![image.png](/images/SSM-Connect/image_14.png)

If the Session Manager tab shows a yellow "Connect" button, your setup is successful.

![image.png](/images/SSM-Connect/image_15.png)

If not, verify the following:
- Ensure the security group configuration is correct; if unsure, assign the default security group to both the endpoint and the EC2 instance.
- Verify that your endpoints and subnets are correctly aligned to the private subnet. Note that the subnet specified for the endpoint in step one is the target for endpoint connection, not the endpoint's location.
- No endpoint is needed for a public subnet.
- Check whether your AMI supports the SSM agent. If it does, reboot the instance; if not, recreate the instance or connect using EC2 Instance Connect to install the agent.

##### Click Connect
![image.png](/images/SSM-Connect/image_16.png)
The interface indicates a successful connection!

---

### Step 4: Connect Using the CLI

The following steps require **AWS CLI v2** installed on your system.

If AWS CLI is not installed, follow these steps:

Refer to: [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- For _Linux_:
    ```bash
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
    ```
- For _Windows_:
    - Download and install AWS CLI from [`https://awscli.amazonaws.com/AWSCLIV2.msi`](https://awscli.amazonaws.com/AWSCLIV2.msi)

- Verify the installation with:
    ```bash
    aws --version
    ```
- Connect via CLI:
    ```bash
    aws ssm start-session --target <instance_id>
    ```
    If the output appears as below, the connection is successful:
    ![image.png](/images/SSM-Connect/image_17.png)

---

### Step 5: Configure VSCode

Now that the EC2 instance is accessible through the CLI, configure it for use with VSCode.

[Follow the same steps as for connecting to an EC2 instance in a public subnet](/vi/2).

In the host file configuration, create tunnels for SSH instead of using the public IP:

```powershell
Host EC2_Instant_Connect_Endpoint
    HostName <your_instance_id>
    User ec2-user 
    IdentityFile "your/path/to/your/key.pem"
    ProxyCommand aws ssm start-session --target %h --document-name AWS-StartSSHSession
```