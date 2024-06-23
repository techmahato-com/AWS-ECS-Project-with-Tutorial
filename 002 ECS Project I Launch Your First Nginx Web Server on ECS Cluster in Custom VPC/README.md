# Launch Your First Nginx Web Server on ECS Cluster in Private Network in Custom VPC
This guide provides a comprehensive overview of setting up and deploying an Nginx web server on ECS in a private network within a custom VPC. Adjust the steps as needed for your specific requirements.

## Step 1: Prerequisites
Before we begin, ensure you have the following:
- Create and Login AWS Console

## Step 2: Create a Custom VPC
- Go to the VPC Dashboard.
- Create VPC:
    - Click on "Create VPC".
    - Name your VPC (e.g., custom-vpc).
    - Set the IPv4 CIDR block (e.g., 10.0.0.0/16).
    - Click "Create VPC".
    - Create.

## Step 3: Create an ECS Cluster
- Navigate to ECS Dashboard:
    - Go to the ECS Dashboard.
- Create Cluster:
    - Click on "Create Cluster".
    - Select "EC2 Linux + Networking" and click "Next Step".
    - Name your cluster (e.g., nginx-private-cluster).
    - For the instance type, select t3a.medium (or any other instance type as - per your requirement).
    - Choose the number of instances you want to launch.
    - Select the VPC and the public subnet created earlier.
    - Click on "Create".

## Step 4: Create a Task Definition
- Navigate to Task Definitions:
    - On the ECS Dashboard, click on "Task Definitions".
    - Click "Create new Task Definition".
- Define Task Definition:
- Select "EC2" and click "Next Step".
- Give your task definition a name (e.g., nginx-task).

- Add Container:
    - Click on "Add container".
    - Name the container (e.g., nginx).
    - For the image, use nginx:latest.
    - Set memory limits (e.g., 128 MiB soft limit).
    - Set port mappings: Host port and container port to 80.
    - Click "Add".

- Save Task Definition:
    - Review your task definition settings.
    - Click "Create".

## Step 5: Create an ECS Service
- Navigate to Services:
    - On the ECS Dashboard, click on your cluster (nginx-private-cluster).
    - Click on "Create".

- Define Service:
    - Choose the launch type as EC2.
    - Name your service (e.g., nginx-service).
    - Select the task definition (nginx-task).
    - Set the number of tasks (desired count) to 1.

- Configure Network:
    - In the Network configuration, select your custom VPC.
    - Choose the private subnet for your instances.
    - Select the security group that allows inbound traffic on port 80 and - outbound traffic.

- Configure Load Balancer (ALB):
    - Navigate to the EC2 Dashboard.
    - Create an Application Load Balancer (ALB).
        - Name your ALB (e.g., nginx-alb).
        - Select the VPC and public subnets.
        - Configure the security group to allow HTTP (port 80) traffic.
        - Create a target group for your ECS service.
        - Register targets (ECS instances) with the target group.
        - Add a listener for HTTP (port 80) traffic to forward to the target group.

- Create Service:
    - Go back to the ECS service creation.
    - In the Load Balancer configuration, select the created target group.
    - Click "Create Service".

## Step 6: Verify the Deployment
- Check ECS Cluster:
    - Go to the ECS Dashboard.
    - Click on your cluster (nginx-private-cluster).
    - Ensure the service is running and the task is in the RUNNING state.
- Access Nginx Web Server:
    - Open a web browser and go to your ALB DNS name (e.g., http://<alb-dns-name>).
    - You should see the default Nginx welcome page.

## Step 7: Clean Up (Optional)
- Delete Service:
    - Go to the ECS Dashboard.
    - Click on your cluster (nginx-private-cluster).
    - Select the service (nginx-service).
    - Click on "Delete" to remove the service.

- Delete Cluster:
    - Go back to the ECS Dashboard.
    - Select your cluster (nginx-private-cluster).
    - Click on "Delete Cluster" to remove the cluster and associated - resources.

- Delete ALB and VPC:
    - Navigate to the EC2 Dashboard.
    - Delete the Application Load Balancer and target group.
    - Delete the custom VPC and associated resources (subnets, route tables, etc.).

Conclusion
You've successfully launched an Nginx web server on an ECS Cluster in a private network within a custom VPC! This guide covered creating a custom VPC, ECS cluster, task definition, and service, and accessing the running Nginx instance through an ALB.

# Become DevOps Pro. & Generate 4 Source of Income || Ask Arbind Sir for Personal Training || Book Free Demo Class @ 8100011825 (WhatsApp Only)