# CloudFormation Basic Network Template Overview

This CloudFormation template is designed to create a Virtual Private Cloud (VPC) along with essential network components, including subnets, an Internet Gateway, and routing configurations. Below is a summary of the main elements of this template:

## Key Components
- **VPC (`MyVPC`)**: A custom VPC with a CIDR block of `10.0.0.0/16`, DNS support, and hostnames enabled. The VPC name can be specified via the `VPCName` parameter.
- **Internet Gateway (`InternetGateway`)**: Facilitates internet access for resources within the VPC.
- **VPC Gateway Attachment (`AttachGateway`)**: Attaches the Internet Gateway to the VPC.
- **Subnets (`PublicSubnet1` and `PublicSubnet2`)**: Two public subnets with CIDR blocks `10.0.1.0/24` and `10.0.2.0/24`, each mapped to different availability zones.
- **Route Table (`RouteTable`)**: A route table associated with the VPC.
- **Public Route (`PublicRoute`)**: Configured to route traffic destined for `0.0.0.0/0` through the Internet Gateway.
- **Subnet Route Table Associations (`SubnetRouteTableAssociation1` and `SubnetRouteTableAssociation2`)**: Associates each subnet with the created route table.

## Outputs
- **VPCId**: The ID of the created VPC.
- **Subnet1Id**: The ID of Public Subnet 1.
- **Subnet2Id**: The ID of Public Subnet 2.

This template is structured for ease of deployment and sets up a basic network infrastructure with public subnets, suitable for hosting resources accessible from the internet.

# CloudFormation ECS Fargate Cluster Template Overview

This CloudFormation template is designed to deploy an ECS (Elastic Container Service) container running on Fargate, integrated with an Application Load Balancer (ALB). The following is a summary of the main components of this template:

## Key Components
- **Parameters**:
  - `MyContainerImage`: The complete Docker container image reference, defaulting to `lroquec/cicd-tests:latest`.

- **Security Groups**:
  - `LoadBalancerSecurityGroup`: Security group allowing HTTP traffic on port 80 to the ALB.
  - `TaskSecurityGroup`: Security group for ECS tasks, allowing traffic on port 5000 from the ALB.

- **Network Components**:
  - **Application Load Balancer (`ApplicationLoadBalancer`)**: Internet-facing ALB with idle timeout set to 60 seconds, linked to imported subnets.
  - **Target Group (`ALBTargetGroup`)**: Defines the target group for routing traffic to ECS tasks, with health checks on the root path `/`.

- **ECS Components**:
  - `ECSCluster`: ECS cluster for hosting the container service.
  - `ECSExecutionRole`: IAM role providing necessary permissions for ECS task execution.
  - `TaskDefinition`: Defines the ECS task with Fargate configuration, using the specified container image and AWS CloudWatch logging.
  - `ECSService`: Configures the ECS service running one task, connected to the ALB for traffic handling.

- **Logging**:
  - `ECSLogGroup`: CloudWatch log group to store logs from the ECS task, retained for 7 days.

- **Outputs**:
  - `LoadBalancerDNS`: The DNS name of the ALB, allowing external access to the service.

This template sets up a scalable infrastructure to deploy a containerized application with secure networking, automatic load balancing, and centralized logging.

# CloudFormation EC2 with Launch Template Overview

This CloudFormation template is designed to deploy an EC2 instance with Apache installed, serving a test HTML page. Below is a summary of the main elements of this template:

## Key Components
- **IAM Role and Instance Profile**:
  - `SSMInstanceRole`: An IAM role for EC2 with `AmazonSSMManagedInstanceCore` policy for Systems Manager access.
  - `SSMInstanceProfile`: An instance profile attached to the EC2 instance, allowing it to use the SSM role.

- **Security Group**:
  - `MyInstanceSecurityGroup`: A security group configured to allow HTTP traffic on port 80 from any IP (`0.0.0.0/0`) and allow outbound traffic.

- **EC2 Instance**:
  - `MyInstance`: An EC2 instance launched using a specified launch template and assigned the SSM instance profile.
  - **AMI ID**: Uses `ami-0ebfd941bbafe70c6` as the base image (Amazon Linux 2).
  - **Instance Type**: Configured as `t2.micro`.

- **Launch Template (`MyLaunchTemplate`)**:
  - Installs Apache (`httpd`) and sets up an HTML test page located at `/var/www/html/index.html`.
  - Uses `cfn-init` for CloudFormation initialization and configuration.

- **User Data**:
  - Executes commands to update packages, install Apache, and enable the service to start on boot.

## Outputs
- **InstanceProfileName**: The name of the created instance profile.
- **PublicIp**: The public IP address of the EC2 instance, allowing access to the hosted web page.

This template sets up a basic web server on an EC2 instance within a specified VPC subnet, configured to be publicly accessible for testing purposes.
