# AWS Highly Available & Scalable Web Application Deployment

This repository provides the architecture and steps to deploy a static HTML/CSS website on AWS, engineered from the ground up for high availability and automatic scaling. This is a foundational project for anyone looking to build robust, production-ready infrastructure in the cloud.


## Final Architecture
The goal is to build the following infrastructure, which ensures traffic is balanced across multiple servers in different data centers, with automatic scaling and failure recovery.

<img width="4425" height="1804" alt="image" src="https://github.com/user-attachments/assets/e7884262-c261-49aa-88c3-d415ecbb5bff" />



## Core Concepts Explained
- High Availability: The Application Load Balancer (ALB) distributes traffic across instances in at least two Availability Zones (AZs). This prevents a single point of failure.

- Dynamic Scaling: The Auto Scaling Group (ASG) monitors CPU load. It adds new instances during traffic spikes to maintain performance and terminates them when demand drops to reduce costs.

- Automated Recovery: The ASG uses health checks from the ALB to detect failing instances. It automatically terminates them and launches healthy replacements.

- Immutable Infrastructure: Instances are configured at launch time via a User Data script. No manual configuration is needed, making every server a predictable and disposable unit.

- Network Security: All resources are deployed within a Virtual Private Cloud (VPC). Security Groups act as stateful firewalls, controlling inbound and outbound traffic to the instances.

- Proactive Monitoring: CloudWatch alarms are configured to trigger on high CPU metrics, sending notifications via SNS so you are aware of scaling events.

## Technology Stack
- Compute: Amazon EC2

- Networking: Amazon VPC, Application Load Balancer (ALB)

- Scaling & Management: Auto Scaling Group (ASG), IAM Role

- Monitoring: Amazon CloudWatch, Amazon SNS


## AWS Services Used
- Compute: Amazon EC2

- Networking: Amazon VPC, Application Load Balancer (ALB)

- Scaling & Management: Auto Scaling Group (ASG), IAM Role

- Monitoring: Amazon CloudWatch, Amazon SNS

## Implementation Steps
1. IAM Role: Create an IAM Role for the EC2 service. This role will be attached to the instances.

2. Launch Template: Define an EC2 Launch Template.

- A MI/Instance Type: Specify the OS `(Amazon Linux 2023)` and instance size `(t2.micro)`.

- Security Group: Attach a security group that allows inbound HTTP traffic (port 80) from `0.0.0.0/0`.

- User Data Script: Provide the shell script that installs Apache and deploys your website files to `/var/www/html/`.

3. Target Group & ALB: Create a Target Group for EC2 instances. Then, create an internet-facing Application Load Balancer and configure its listener to forward traffic to this Target Group.

4. Auto Scaling Group: Create the ASG.

- Link it to your Launch Template and the ALB's Target Group.

- Set the desired, minimum, and maximum number of instances.

- Define a target tracking scaling policy based on average CPU utilization.

5. Alerting: Create an SNS topic and subscribe your email. Create a CloudWatch alarm to monitor the ASG's CPU utilization and send a notification to the SNS topic when triggered.

## Live Demo
You can view the Live page from:[Here](http://webapp-alb-949908307.us-east-1.elb.amazonaws.com/)

Note:The site may be offline 
