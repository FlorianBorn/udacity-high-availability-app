# udacity-high-availability-app
Udacity Cloud DevOps ND / High Availability App / Cloudformation
This repository was created as submission to Udacities 'Deploy a high-availability web app using CloudFormation' Project (Cloud DevOps Nanodegree).

# Introduction
The goal of this project is to deploy an application in an automated fashion using AWS cloudformation.
The resulting infrastructure can be discarded and rebuild at any time.

In this scenario an app called Udagram is supposed to be deployed in a high-available and automated fashion.
The application data is located in a S3 Bucket.

To-Do's:
* create an Infrastructure Diagram
* create the S3 Bucket and place the application code in it (since there is no bucket yet)
* create the neccessary Cloudformation Scripts

# Infrastructure Diagram
![alt text][architecture]

[architecture]: infrastructure-diagram.png "Architecture Diagram"

# S3 Bucket
The S3 Bucket is needed to hold the application data (udacity.zip). On start each EC2-Instance will download and unpack this data.
The S3 Bucket can simply be created via the management console. Afterwards the data can be uploaded to the bucket.

In order to make its data accessible to other ressources, a bucket policy must be created.
[The following rule grants EC2 instances access to all actions to a specific bucket and to all the objects stored in it.](https://aws.amazon.com/de/premiumsupport/knowledge-center/s3-instance-access-bucket/) 
(This policy is probably to open and could be narrowed down)
```json
{
    "Version": "2012-10-17",
    "Id": "Policy1564815775429",
    "Statement": [
        {
            "Sid": "Stmt1564815768845",
            "Effect": "Allow",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::<YOUR-BUCKET-NAME>",
                "arn:aws:s3:::<YOUR-BUCKET-NAME>/*"
            ]
        }
    ]
}
```
# CloudFormation
All required scripts are available in this repository. 
network.yml sets up the required network infrastructure (VPC, Subnets, Gateways, Routing, etc.). 
servers.yml will set up all components which are required to deploy the app (Load Balancer, Auto Scaling Group, Security Groups, Policies (for accessing the S3 Bucket), etc.). 
Furthermore, additional bastion hosts are deployed to grant SSH access to the EC2 instances in the private subnet.
In order to run these scripts AWS CLI is needed.

## Bastion Hosts
![alt text][ssh-agent-forwarding]

[ssh-agent-forwarding]: ssh-agent-forwarding.png "ssh-agent-forwarding"

[Bastion Hosts](https://docs.aws.amazon.com/quickstart/latest/linux-bastion/architecture.html) are used to connect to internal infrastructure without exposing it to the Internet. 
To connect from the local system to the internal EC2 instances, ssh-agent-forwarding is used. For this, 2 Key-Pairs are needed. The private keys are stored on the local system while the public keys are distributet over the bastion hosts and the EC2 instances (or rather the autoscaling group)




To access instances which
