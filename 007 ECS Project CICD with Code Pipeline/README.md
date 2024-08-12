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


# CI/CD Pipeline for ECS with AWS CodePipeline

This repository demonstrates setting up a Continuous Integration and Continuous Delivery (CI/CD) pipeline using AWS CodePipeline for deploying applications to ECS (Elastic Container Service) with separate staging and production environments, including manual approval for production deployments.

## Table of Contents
1. [Introduction to CI/CD](#introduction-to-cicd)
2. [Pre-requisites - ECS Services Setup](#pre-requisites---ecs-services-setup)
3. [Create CodeCommit Repository](#create-codecommit-repository)
4. [Create `buildspec.yml` for CodeBuild](#create-buildspecyml-for-codebuild)
5. [Create CodePipeline](#create-codepipeline)
6. [Modify `index.html` and Redeploy](#modify-indexhtml-and-redeploy)
7. [Add Manual Approval Stage](#add-manual-approval-stage)
8. [Deploy to Production ECS Service](#deploy-to-production-ecs-service)

## Introduction to CI/CD

Continuous Integration (CI) and Continuous Delivery (CD) enable developers to integrate and deliver code changes more frequently and reliably through automation. CI involves automatically testing and integrating code changes into a shared repository, while CD extends CI by automatically deploying code changes to a production-like environment.

## Pre-requisites - ECS Services Setup

### 1. Create ECS Task Definition

- **Task Definition Name:** `ecs-cicd-nginx`
- **Container Name:** `ecs-cicd-nginx`
  - *Important:* Ensure the container name matches the name specified in your `buildspec.yml` file.
- **Image:** `stacksimplify/nginxapp2:latest`

### 2. Create ECS Services

- **Staging ECS Service:**
  - **Service Name:** `staging-ecs-cicd-nginx-svc`
  - **Number of Tasks:** 1

- **Production ECS Service:**
  - **Service Name:** `prod-ecs-cicd-nginx-svc`
  - **Number of Tasks:** 1

## Create CodeCommit Repository

1. **Repository Name:** `ecs-cicd-nginx`
2. **Git Credentials:** Generate git credentials from the IAM service and store them securely.

### Clone the Repository
```bash
git clone https://git-codecommit.ap-south-1.amazonaws.com/v1/repos/ecs-cicd-nginx

Add Necessary Files
Copy the following files to your local repository:

Dockerfile
index.html
Commit and Push to CodeCommit

git status
git add .
git commit -m "1-Added Dockerfile and index.html"
git push


Verify the commit on the CodeCommit repository via the AWS Management Console.

Create buildspec.yml for CodeBuild
1. Create an ECR Repository
Repository Name: ecs-cicd-nginx
Note: Record the full name of the ECR repository.
2. Create and Update buildspec.yml
Create a buildspec.yml file in your local ecs-cicd-nginx folder with the following content:

3. Commit and Push buildspec.yml
git status
git add .
git commit -m "2-Added buildspec.yml"
git push


Create CodePipeline
Create a CodePipeline:

Connect your CodeCommit repository as the source.
Use the buildspec.yml in the CodeBuild stage.
Update CodeBuild Role:

Attach the policy AmazonEC2ContainerRegistryFullAccess to allow CodeBuild to push images to ECR.
Test Deployment:

Access the static HTML page served by the Nginx container to verify deployment.
Modify index.html and Redeploy
1. Make Changes
Update the index.html file with new content (e.g., "V2 Deployment").

2. Commit and Push Changes
git status
git commit -m "V2 Deployment"
git push

bash
git status
git commit -m "V2 Deployment"
git push

3. Monitor CodePipeline
Ensure the pipeline automatically triggers and deploys the updated application.

4. Test Deployment
Verify the changes by accessing the updated static HTML page.

Add Manual Approval Stage
1. Create SNS Topic
Set up an SNS topic for sending approval notifications.
Confirm the email subscription to receive notifications.
2. Edit Pipeline
Add a manual approval stage in the pipeline after the staging deployment.
Deploy to Production ECS Service
1. Edit Pipeline
Add a deployment stage for the production ECS service after the manual approval stage.
2. Test Final Deployment
After manual approval, verify that the application is deployed to the production environment.

