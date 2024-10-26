# Secure AWS VPC Architecture with Private Subnets and NAT Gateway for Scalable Web Services

## Introduction
This project demonstrates a secure, scalable architecture for web services on AWS using a Virtual Private Cloud (VPC). The setup is ideal for production environments, where resiliency and security are prioritized. It includes private subnets, NAT gateways, Auto Scaling, an Application Load Balancer (ALB), and a Bastion host. Resources are distributed across two Availability Zones (AZs) for high availability and fault tolerance.

## Table of Contents
- [Introduction](#introduction)
- [Architecture Overview](#architecture-overview)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
  - [1. VPC Creation](#1-vpc-creation)
  - [2. Auto Scaling Group Configuration](#2-auto-scaling-group-configuration)
  - [3. Target Group for HTTP Requests](#3-target-group-for-http-requests)
  - [4. Application Load Balancer Setup](#4-application-load-balancer-setup)
  - [5. Bastion Host Configuration](#5-bastion-host-configuration)
  - [6. Web Server Deployment](#6-web-server-deployment)
- [Testing the Setup](#testing-the-setup)
- [Troubleshooting](#troubleshooting)
- [Contributors](#contributors)
- [License](#license)

## Architecture Overview
The architecture features:
- **VPC with Public and Private Subnets**: Each AZ has both a public subnet with a NAT gateway and private subnets for secure instance hosting.
- **Auto Scaling Group**: Manages the automatic scaling of EC2 instances to meet demand.
- **Application Load Balancer**: Directs traffic to instances within private subnets.
- **Bastion Host**: Provides secure SSH access to instances in private subnets.

  ![image](https://github.com/user-attachments/assets/b934c80e-17d0-4278-8c55-049bc4d1cfcd)


## Prerequisites
- **AWS Account** with appropriate permissions to create VPCs, subnets, NAT gateways, load balancers, and other necessary resources.
- **AWS CLI** and **AWS Management Console** access.
- Familiarity with basic **networking concepts** and **EC2 configuration**.

## Setup Instructions

### 1. VPC Creation
1. **Navigate to VPC Console**: Open the [Amazon VPC Console](https://console.aws.amazon.com/vpc/).
2. **Create VPC**:
   - Choose "Create VPC and more" on the dashboard.
   - Set Name, IPv4 CIDR block, and select IPv6 if required.
3. **Configure Subnets**:
   - Select two Availability Zones, each with one public and one private subnet.
4. **NAT Gateways**:
   - Select 1 NAT gateway per AZ for redundancy.
  
     ![image](https://github.com/user-attachments/assets/8e2ede47-8f12-4003-b391-15a0374e74ee)

### 2. Auto Scaling Group Configuration
1. **Create Launch Template**:
   - Go to EC2 Dashboard > Launch Templates, click "Create launch template."
   - Configure template with a suitable AMI (e.g., Ubuntu Server 20.04 LTS), instance type, key pair, and security group.
2. **Create Auto Scaling Group**:
   - Navigate to Auto Scaling Groups, click "Create Auto Scaling group."
   - Set desired, minimum, and maximum instance counts.
   - Link to the launch template and select the VPC/subnets.
  
   ![image](https://github.com/user-attachments/assets/7462b975-a6b1-4bbb-a13d-926214cad506)

   ![image](https://github.com/user-attachments/assets/3aee8c3a-53c3-459f-b294-19f13eb952fd)

### 3. Target Group for HTTP Requests
1. **Navigate to Target Groups**:
   - Choose "Create target group," set type to Instances.
   - Define settings (HTTP on port 80) and link to the VPC.
   - Register EC2 instances as targets.
  
     ![image](https://github.com/user-attachments/assets/3a86cd2d-84e7-4a3f-ada6-dcdbad175adc)

### 4. Application Load Balancer Setup
1. **Create Application Load Balancer**:
   - Go to Load Balancers, select Application Load Balancer.
   - Define listener settings and link to public subnets.
   - Choose a security group allowing inbound traffic on port 80.
   - Associate with the target group created earlier.
  
     ![image](https://github.com/user-attachments/assets/36818521-20e9-493f-9606-5a82c3368159)

### 5. Bastion Host Configuration
1. **Launch a Bastion Host**:
   - Create an EC2 instance in a public subnet using an Ubuntu AMI, t2.micro instance type.
   - Enable public IP and allow SSH access in the security group.
2. **Connect to Bastion Host**:
   - Use SSH to connect to the Bastion host, and then access private subnet instances from there.

### 6. Web Server Deployment
1. **Deploy Simple Web Server**:
   - SSH into one of the instances in the private subnet.
   - Create a simple HTML page:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
     <title>AWS Project</title>
     </head>
     <body>
     <h1>This is an AWS project</h1>
     </body>
     </html>
     ```
   - Start a web server on port 8000:
     ```bash
     python3 -m http.server 8000
     ```
![image](https://github.com/user-attachments/assets/414c248d-c5d9-441a-99d0-abc346b9bc75)


## Testing the Setup
- **Access via Load Balancer**: Copy the DNS name of the Load Balancer, open it in a browser, and confirm the HTML page displays.
- **Instance Monitoring**: Check the Auto Scaling group in the EC2 Dashboard to ensure instances are being launched and terminated as expected.

## Troubleshooting
- **Connectivity Issues**: Verify that the security groups allow appropriate inbound/outbound traffic, and ensure NAT gateways are properly configured for private subnets.
- **Scaling Issues**: Review Auto Scaling policies and check for any quota limitations on instance counts.

## Contributors
- Project by [Your Name or Team Name]

## License
This project is licensed under the MIT License.
