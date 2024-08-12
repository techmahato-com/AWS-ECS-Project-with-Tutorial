## Create VPC
## Create Stage Cluster
## Create CodeCommit Repo & Upload Code
## Create ECR | Build Code and Test in Local || Push Build to ECR
## Create Task Defination
## Create Service with Load Balancer and Test
## Create Buildspec File and upload to CodeCommit
## Build the Code
## Create Pipeline to Deploy Code in Stage
## 


# Continuous Integration and Continuous Delivery (CI/CD) Pipeline for ECS
## Step 1: Introduction to CI/CD
CI/CD is a methodology that enables developers to deliver code changes more frequently and reliably through automation. Continuous Integration (CI) involves automatically testing and integrating code changes into a shared repository. Continuous Delivery (CD) extends CI by automatically deploying code changes to a production-like environment.

Refer to the my youtube video for a detailed hands-on demonstration.

## Step 2: Pre-requisites - Create Staging and Production Services in ECS
### Create ECS Task Definition
    - Name: ecs-cicd-nginx
    - Container Name: ecs-cicd-nginx
        - Important Note: Ensure the container name matches what is specified in your buildspec.yml file.
    - Image: stacksimplify/nginxapp2:latest
### Create ECS Services
    - Staging ECS Service
        - Name: staging-ecs-cicd-nginx-svc
        - Number of Tasks: 1
    - Production ECS Service
        - Name: prod-ecs-cicd-nginx-svc
        - Number of Tasks: 1

## Step 3: Create a CodeCommit Repository
    - Repository Name: ecs-cicd-nginx
    - Git Credentials: Generate git credentials from the IAM service and store them securely.

Clone the Repository
        git clone https://git-codecommit.ap-south-1.amazonaws.com/v1/repos/ecs-cicd-nginx
Add Necessary Files
Copy the following files from the course section 06-CICD-ContinuousIntegration-ContinuousDelivery to your local repository:

    - Dockerfile
    - index.html
Commit and Push to CodeCommit

        git status
        git add .
        git commit -am "1-Added Dockerfile and index.html"
        git push
V
erify the commit on the CodeCommit repository via the AWS Management Console.

## Step 4: Create buildspec.yml for CodeBuild
### Create an ECR Repository
        - Repository Name: ecs-cicd-nginx
        - Note: Record the full name of the ECR repository.

### Create and Update buildspec.yml
Create a buildspec.yml file in your local ecs-cicd-nginx folder and update it as follows:

        - REPOSITORY_URI: Replace with the complete ECR Repository URI.
        - Container Name: Ensure the container name matches ecs-cicd-nginx.


Commit and Push buildspec.yml

        git status
        git add .
        git commit -am "2-Added buildspec.yml"
        git push

## Step 5: Create CodePipeline

### Create a CodePipeline:

        - Connect your CodeCommit repository as the source.
        - Use the buildspec.yml in the CodeBuild stage.

###  Update CodeBuild Role:

        - Attach the policy AmazonEC2ContainerRegistryFullAccess to allow CodeBuild to push images to ECR.

### Test Deployment:

        - Access the static HTML page served by the Nginx container to verify deployment.

## Step 6: Modify index.html and Redeploy
### Make Changes:

        - Update the index.html file with new content (e.g., "V2 Deployment").

### Commit and Push Changes:

        - git status
        - git commit -am "V2 Deployment"
        - git push
### Monitor CodePipeline:

        - Ensure the pipeline automatically triggers and deploys the updated application.

### Test Deployment:

        - Verify the changes by accessing the updated static HTML page.

## Step 7: Add Manual Approval Stage in CodePipeline
### Create SNS Topic:
        - Set up an SNS topic for sending approval notifications.
        - Confirm the email subscription to receive notifications.

### Edit Pipeline:

        - Add a manual approval stage in the pipeline after the staging deployment.

## Step 8: Deploy to Production ECS Service

### Edit Pipeline:
        - Add a deployment stage for the production ECS service after the manual approval stage.
### Test Final Deployment:

        - After manual approval, verify that the application is deployed to the production environment.
