# Amazon ECS Fargate CICD Terraform
Docker image deployment to ECS with Terraform and CICD

## OVERVIEW
This project provisions a secure AWS VPC, deploys a Springboot application on ECS Fargate

## ARCHITECTURE
- Terraform - infrasture as code
- Virtual Private Cloud (VPC) - with two public and two private subnets
- VPC Endpoint - allows private AWS service communication
- Internet Gateway - allows internet access for public subnets
- Security Groups - allows least priviledge network access
- Route 53 - routes end users to internet applications by managing DNS records
- Elastic Container Registry (ECR) - stores the Docker image
- IAM - handles user, roles or services permissions
- OpenID Connect (OIDC) - enables external entities to access AWS resources using temporary,    short-lived credentials
- Elastic Container Service - orchestrates the Docker containers
- Load Balancer - distributes traffic across Availability Zones
- Application Autoscaling - automatically adjusts the capacity to maintain steady performance
- Amazon S3 - manages Terraform backend state
- Amazon Cloudwatch - displayes logs for monitoring 

## DEPLOYMENT
- Provisioned using Terraform
- Deployment automated through GitHub Actions

## DIAGRAM
![alt text](<images/architecture.png>)

## LEARNING OUTCOMES
- Designed a highly available, secure VPC  
- Automated ECS Fargate provisioning  
- Integrated AWS OIDC Authentication with Github actions
- Multi-staging deployments

## Provisioned resources

The following screenshots demonstrate the successful provisioning of AWS infrastructure using Terraform, triggered via a GitHub Actions CI/CD pipeline.

**GitHub Actions Deployment Trigger**

Git push triggering the CICD deployment execution in GitHub Actions

**Infrastructure Provisioning Via GitHub Actions**

Successful end-to-end infrastructure provisioning executed through GitHub Actions using Terraform.

**Terraform Remote State Backend (Amazon S3)**

Terraform state stored remotely in Amazon S3 backend for state consistency and collaboration

**Elastic Container Repository Provisioning**

ECR created successfully

**Virtual Private Cloud (VPC)**

VPC created successfully with public and private subnets as defined in Terraform.

**Security Groups Configurations**

Security Groups provisioned successfully for Load Balncer, ECS Tasks and VPC Endpoints

**ECS Cluster with service and tasks**

ECS Cluster provisioned successfully with service and tasks 

**Application Load Balancer (ALB)**

Application Load Balancer created to distribute traffic across ECS tasks.

**Target Group Health Status**

Target group showing a healthy EC2 instance registered behind the load balancer.

**Application Auto Scaling for ECS**

Application AutoScaling configured to manage ECS tasks.

VPC Endpoints
- This architecture removes NAT Gateway to reduce cost 
by using VPC endpoints for private AWS service communication.