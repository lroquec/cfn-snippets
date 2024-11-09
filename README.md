# CloudFormation Snippets Overview

This repository contains multiple CloudFormation templates designed to help set up various AWS resources efficiently. Below is an overview of each template included in the repository:

## 1. VPC and Networking Template

**Description**: This template creates a Virtual Private Cloud (VPC) along with subnets and essential network components.

### Key Components
- **VPC (`MyVPC`)**: Custom VPC with a CIDR block of `10.0.0.0/16`, DNS support, and hostnames enabled.
- **Internet Gateway (`InternetGateway`)**: Enables internet access for resources within the VPC.
- **Subnets**: Two public subnets (`PublicSubnet1` and `PublicSubnet2`) with CIDR blocks `10.0.1.0/24` and `10.0.2.0/24` mapped to different availability zones.
- **Route Table**: Configured with a public route (`0.0.0.0/0`) to the Internet Gateway.
- **Subnet Associations**: Links subnets to the route table for public access.

### Outputs
- **VPCId**: ID of the created VPC.
- **Subnet1Id**: ID of `PublicSubnet1`.
- **Subnet2Id**: ID of `PublicSubnet2`.

## 2. ECS Container with ALB Template

**Description**: This template deploys an ECS container running on Fargate, integrated with an Application Load Balancer (ALB).

### Key Components
- **Parameters**:
  - `MyContainerImage`: Specifies the Docker container image (default: `lroquec/cicd-tests:latest`).
- **ALB and Security**:
  - `ApplicationLoadBalancer`: Internet-facing ALB for traffic handling.
  - `LoadBalancerSecurityGroup`: Allows HTTP traffic on port 80.
  - `ALBTargetGroup` and `ALBListener`: Configures the target group and listener for routing.
- **ECS Configuration**:
  - `ECSCluster` and `TaskDefinition`: Sets up an ECS cluster and task running the container.
  - `ECSService`: Deploys a service using the Fargate launch type.
- **Logging**:
  - `ECSLogGroup`: Stores ECS task logs in CloudWatch.

### Outputs
- **LoadBalancerDNS**: The DNS name of the ALB.

## 3. EC2 Instance with Apache Template

**Description**: This template provisions an EC2 instance with Apache installed and serves a basic test HTML page.

### Key Components
- **IAM Role and Instance Profile**:
  - `SSMInstanceRole` and `SSMInstanceProfile`: Provides SSM access for the EC2 instance. It also provides Get Access to S3 files needed deployment artifacts.
- **Security Group**:
  - `MyInstanceSecurityGroup`: Allows HTTP traffic on port 80.
- **EC2 Instance**:
  - `MyInstance`: Launches an `t2.micro` instance using a defined launch template.
- **Launch Template (`MyLaunchTemplate`)**:
  - Sets up Apache and creates an HTML page in `/var/www/html/index.html`.
  - User data installs Apache and starts the service on boot.

### Outputs
- **InstanceProfileName**: Name of the created instance profile.
- **PublicIp**: Public IP address of the EC2 instance.

## Summary

These CloudFormation templates provide quick and efficient ways to set up basic AWS resources such as VPCs, ECS services with load balancers, and EC2 instances with pre-installed software. Use them as a starting point for creating and managing AWS infrastructure tailored to your needs.
