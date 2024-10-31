---
title: "Create an instance from AMI built from pipeline"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3 </b> "
---

#### Create AMI from pipeline

1. Select the pipeline you just created. In the Action box, select Run pipeline:

![console](/images/img_sec7/3/1/1.png)

2. Click View details to track details:

![console](/images/img_sec7/3/1/2.png)
You will see the Image status as Testing.

3. Click the log stream link to follow the CloudWatch log stream of the pipeline.

Phase test, step **TestWhetherAppDeploy**:
![console](/images/img_sec7/3/1/3.png)

Phase **RebootTest**:

![console](/images/img_sec7/3/1/3b.png)
If there's an error, the exit code is 1; if there's no error, the exit code is 0. Finally, the test instance has no errors.

4. Go back to the running pipeline. When the AMI build is complete, the image status changes to Available. Click on the created image version (the image below shows version 1.0.4/1):
![console](/images/img_sec7/3/1/4.png)

5. Observe the image created from EC2 Image Builder. Click on the AMI link:
![console](/images/img_sec7/3/1/5.png)

6. View the AMI from the EC2 page.
![console](/images/img_sec7/3/1/6.png)
The AMI was successfully created and passed all tests in the pipeline process without any errors.

#### Create an instance from the created AMI
1. Go to the AMI page in the EC2 console. Select the AMI you just created. Click **Launch instance from AMI**.
![console](/images/img_sec7/3/2/1.png)

2. While configuring the instance, select the instance type. For cost-saving, choose **t2.micro**.

3. No key pair is needed.
![console](/images/img_sec7/3/2/3.png)

4. Select the VPC and security group from **VSC**, choose the private subnet, and ensure there is no public IP.
![console](/images/img_sec7/3/2/4.png)

5. Provide the IAM instance profile from **VSC** to automatically install the SSM agent.
![console](/images/img_sec7/3/2/5.png)

6. Select Create instance.

7. Wait until the instance status is **Running**.
![console](/images/img_sec7/3/2/7.png)

8. Access the VSCode web app using Session Manager through port forwarding.
```
aws ssm start-session ^
--target i-06ae65824244ff79b ^
--document-name AWS-StartPortForwardingSessionToRemoteHost ^
--parameters host="10.0.132.106",portNumber="8080",localPortNumber="8080"
```
![console](/images/img_sec7/3/2/8.png)
Access the EC2 instance successfully!