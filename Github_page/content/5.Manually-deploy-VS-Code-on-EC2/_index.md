---
title : "Manually deploy VS Code on EC2"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
(extended from section 4, no need to use CDK)

## 4.1. Environment setup

### 4.1.1. VPC

Requirements:

- Private subnet to set EC2 instance (minimum 1)
- 1 NAT Gateway for instance to access the internet
- Minimum 1 Public subnet to set Internet Gateway

For convenience, I use the “VPC and more” feature in “Create VPC”:

![Untitled](/images/part4/4.1.1-1.png)

Resource map:

![Untitled](/images/part4/4.1.1-2.png)

Result:

![Untitled](/images/part4/4.1.1-3.png)

### 4.1.2. Create Role and Policy for SSM

### Step 1: Create policy

Create a JSON Policy, can be named VSCodeBastionHostInstanceRoleDefaultPolicy

```jsx
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

![Untitled](/images/part4/4.1.2-step1.png)

### Step 2: Create a role and add the newly created policy to the role

Name the role as VscodeOnEc2ForPrototyping

![Untitled](/images/part4/4.1.2-step2.png)
Role used for IAM instance profile

### 4.1.3. Create security group for EC2

If your EC2 instance is not a public application, then according to best practice, there is no need for inbound rule, because we only access EC2 instance via Session Manager port forwarding.

Name the security group VSC-SG

![Untitled](/images/part4/4.1.3-step1.png)

![Untitled](/images/part4/4.1.3-step2.png)

## 4.2 Deployment

### 4.2.1. Create EC2 instance

### Step 1: Select AMI as Amazon Linux 2023, instance type as t3.medium

![Untitled](/images/part4/4.2.1-step1.png)

### Step 2: Select the newly created VPC → any private subnet

### Step 3: Make sure there is no public IP

### Step 4: Select security group VSC-SG

![Untitled](/images/part4/4.2.1-step4.png)

### Step 5: Configure volume 100GB

![Untitled](/images/part4/4.2.1-step6.png)

### Step 6: Select IAM instance profile named “VscodeOnEc2ForPrototyping” created from before ![Untitled](/images/part4/4.2.1-step6.png)

### Step 7: In user-data section, paste the following code: ```jsx #!/bin/bash #!/bin/bash sudo rpm --import https://packages.microsoft.com/keys/microsoft .asc sudo sh -c 'echo -e "[code] name=Visual Studio Code baseurl=https://packages.microsoft.com/yumrepos/vscode enabled=1 gpgcheck=1 gpgkey=https://packages.microsoft. com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo' # https://github.com/amazonlinux/amazon-linux-2023/issues/397 sleep 10 sudo yum install -y code git sudo tee /etc/systemd/system/code-server.service <<EOF [Unit] Description=Start code server [Service] ExecStart=/usr/bin/code serve-web --port 8080 --host 0.0.0.0 --without-connection-token Restart=always Type=simple User=ec2-user [Install] WantedBy = multi- user.target EOF sudo systemctl daemon-reload sudo systemctl enable --now code-server # Install Node.js sudo -u ec2-user -i <<EOF curl -o- https://raw.githubusercontent.com/nvm- sh/nvm/v0.39.7/install.sh | bash
source .bashrc
nvm install 20.11.0
nvm use 20.11.0
EOF
```

Purpose: deploy VS-Code on web port 8080 of EC2 instance

### Step 8: Finally, select “Create instance”.

After a few minutes, make sure the instance has been successfully initialized

![Untitled](/images/part4/4.2.1-step8.png)

### Step 9: Connect to the instance by selecting Connect → Session Manager

Make sure SSM agent has been installed by connecting to the instance via Session Manager

![Untitled](/images/part4/4.2.1-step9.png)

When the configuration is successful:

![Untitled](/images/part4/4.2 .1-step9-1.png)

### Step 10: Check the web app has been deployed successfully with the command

```jsx
curl localhost:8080
```

Once the web has been deployed, we can get the resources of the page open on the port 8080: ![Untitled](/images/part4/4.2.1-step9-2.png) ### 4.2.2. Access VS Code deployed on EC2 from SSM via port forwarding

Replace <instance-ID> and <Private-ID> in the script below with the private IP and instance ID of the EC2 instance, respectively

### Connect from Linux (note must have GUI on Linux machine):

```jsx
aws ssm start-session \
--target <instance-ID> \
--document-name AWS-StartPortForwardingSessionToRemoteHost \
--parameters '{"host":["<Private- IP>"],"portNumber":["8080"], "localPortNumber":["8080"]}'
```

![Untitled](/images/part4/4.2.2-1.png)

### Connect from Windows:

```jsx
aws ssm start-session ^
--target <instance-ID> ^ --document-name AWS-StartPortForwardingSessionToRemoteHost ^ --parameters host="<Private-IP>",portNumber="8080",localPortNumber="8080" ``` ![Untitled](/images/ part4/4.2.2-2.png) At this time, access [localhost:8080](http://localhost:8080) on the browser: ![Untitled](/images/part4/4.2.3-3.png) ## 4.2. Check the performance: Youtube link: https://youtu.be/6DxtEsX6gvY