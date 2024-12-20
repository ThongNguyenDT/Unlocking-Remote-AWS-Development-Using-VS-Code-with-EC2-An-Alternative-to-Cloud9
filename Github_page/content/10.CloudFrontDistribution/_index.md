---
title: "Hosting Code Server on EC2 and CloudFront Distribution"
date :  "`r Sys.Date()`" 
weight : 9
chapter : false
pre : " <b> 9. </b> "
---


### Create a VPC

- Requirements: 
  - Private subnet for hosting an EC2 instance (minimum 1)
  - 1 NAT Gateway to allow the instance to access the internet
  - At least 1 Public subnet for placing the Internet Gateway

- Use the "VPC and more" feature in Create VPC to automatically create the necessary subnets and gateways.

![cloudfrontdistribution_1](/images/cloudfrontdistribution/cloudfrontdistribution_1.png)


![cloudfrontdistribution_2](/images/cloudfrontdistribution/cloudfrontdistribution_2.png)

- Resource map:

![cloudfrontdistribution_3](/images/cloudfrontdistribution/cloudfrontdistribution_3.png)

- Once completed, the VPC and subnet will be ready for use.

![cloudfrontdistribution_6](/images/cloudfrontdistribution/cloudfrontdistribution_6.png)

### CloudFormation

Link download file: [code-server-stack.yaml](https://github.com/aws-samples/code-server-setup-with-cloudformation/blob/main/code-server-stack.yaml) 

By default, the provided template supports the following regions:
| Region        | CloudFront Prefix List ID |
|----------------|---------------------------|
| us-west-2      | pl-82a045eb               |
| us-east-1      | pl-3b927c52               |
| ap-southeast-1 | pl-31a34658               |

To deploy in a different region, you need to update the CloudFront Prefix List ID mapping. Check [the documentation](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-aws-managed-prefix-lists.html) for more details on finding the CloudFront prefix list ID for your region.

```
Mappings:
  CloudFrontPrefixListIdMappings:
    us-west-2:
      PrefixListId: "pl-82a045eb"
    us-east-1:
      PrefixListId: "pl-3b927c52"
    ap-southeast-1:
      PrefixListId: "pl-31a34658"
    <YOUR_SELECTED_REGION>:
      PrefixListId: <CLOUDFRONT_PREFIX_LIST_ID_FOR_THE_SELECTED_REGION>
```
Create a new stack using the CloudFormation template.

![cloudfrontdistribution_4](/images/cloudfrontdistribution/cloudfrontdistribution_4.png)

![cloudfrontdistribution_5](/images/cloudfrontdistribution/cloudfrontdistribution_5.png)


- Create stack:

![cloudfrontdistribution_7](/images/cloudfrontdistribution/cloudfrontdistribution_7.png)


![cloudfrontdistribution_8](/images/cloudfrontdistribution/cloudfrontdistribution_8.png)

- Stack name: ``code-server-stack``
- For SubnetID, VPCID: Select the ID created above

![cloudfrontdistribution_9_](/images/cloudfrontdistribution/cloudfrontdistribution_9_.png)

![cloudfrontdistribution_9](/images/cloudfrontdistribution/cloudfrontdistribution_9.png)

- VPCID:

![cloudfrontdistribution_10](/images/cloudfrontdistribution/cloudfrontdistribution_10.png)

- SubnetID:

![cloudfrontdistribution_11](/images/cloudfrontdistribution/cloudfrontdistribution_11.png)

- Click next, submit:

![cloudfrontdistribution_13](/images/cloudfrontdistribution/cloudfrontdistribution_13.png)

![cloudfrontdistribution_14](/images/cloudfrontdistribution/cloudfrontdistribution_14.png)

- Wait about 5 minutes for the resource creation process to complete.

![cloudfrontdistribution_15](/images/cloudfrontdistribution/cloudfrontdistribution_15.png)

![cloudfrontdistribution_17](/images/cloudfrontdistribution/cloudfrontdistribution_17.png)

- View EC2:

![cloudfrontdistribution_16](/images/cloudfrontdistribution/cloudfrontdistribution_16.png)


![cloudfrontdistribution_18](/images/cloudfrontdistribution/cloudfrontdistribution_18.png)

![cloudfrontdistribution_19_](/images/cloudfrontdistribution/cloudfrontdistribution_19_.png)



### Connect to the Instance using Connect → Session Manager
- Go to EC2, select Connect → Session Manager to connect to the instance.

![cloudfrontdistribution_19](/images/cloudfrontdistribution/cloudfrontdistribution_19.png)

- Verify that the web app has been successfully deployed by running the command:
```
curl localhost:8080
```

![cloudfrontdistribution_20](/images/cloudfrontdistribution/cloudfrontdistribution_20.png)

#### Configure AWS CLI

- Install and configure the AWS CLI to connect to your AWS account.
- Run the configuration command:
```bash
aws configure
```

- This command will prompt you to provide the following information:

  - **AWS Access Key ID**: Created in the AWS Management Console.
  - **AWS Secret Access Key**: Created along with the Access Key ID.
  - **Default region name**: The default region you want to use (e.g., `ap-southeast-1`).
  - **Default output format**: The default output format (`json`, `text`, or `table`).

#### Install Session Manager Plugin

- **Windows**:
    1. Go to the [Session Manager Plugin download page](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html) on the AWS website.
    2. Download the installer for Windows.
    3. Run the installer and follow the instructions to complete the installation.

        Reference: [Install plugin windows](https://docs.aws.amazon.com/systems-manager/latest/userguide/install-plugin-windows.html) 

- **MacOS**:
    1. Use Homebrew to install the plugin:
        
        ```bash
        brew install --cask session-manager-plugin
        ```
        
- **Linux**:
    1. Download and install the plugin with the following command:
        
        ```
        curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
        sudo dpkg -i session-manager-plugin.deb
        ```


#### Verify the Installation

After installation, you can verify that the plugin has been successfully installed by running the following command:
```
session-manager-plugin --version
```

### Access the Deployed VS Code on EC2 from SSM via Port Forwarding

Use the following script, replacing <instance-ID> and <Private-IP> with your EC2 instance ID and IP.
```
aws ssm start-session \
  --target <instance-ID> \
  --document-name AWS-StartPortForwardingSessionToRemoteHost \
  --parameters host="10.0.128.88",portNumber:"8080",localPortNumber:"8080"
```

![cloudfrontdistribution_21](/images/cloudfrontdistribution/cloudfrontdistribution_21.png)

- VSCode interface:

![cloudfrontdistribution_22](/images/cloudfrontdistribution/cloudfrontdistribution_22.png)


{{% notice note %}}
The content of the post is referenced from the article: https://github.com/aws-samples/code-server-setup-with-cloudformation

{{% /notice %}}