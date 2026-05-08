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

**Terraform Remote State Backend (Amazon S3)**

Terraform state stored remotely in Amazon S3 backend for state consistency and collaboration

![alt text](<images/terraform_backend_state.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

## Provisioned resources

The following screenshots demonstrate the successful provisioning of AWS infrastructure using Terraform, triggered via a GitHub Actions CI/CD pipeline.

**Infrastructure Provisioning Via GitHub Actions**

Successful end-to-end infrastructure provisioning executed through GitHub Actions using Terraform.

![alt text](<images/github_actions_deploy.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**Elastic Container Repository**

ECR created successfully

![alt text](<images/amazon_ecr.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**Virtual Private Cloud (VPC)**

VPC created successfully with public and private subnets as defined in Terraform.
![alt text](<images/vpc_details.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**Security Groups Configurations**

Security Groups provisioned successfully for Load Balancer, ECS Tasks and VPC Endpoints

![alt text](<images/security_groups.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**ECS Cluster with service and tasks**

ECS Cluster provisioned successfully with service and tasks 

![alt text](<images/ecs_cluster_service.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**Application Load Balancer (ALB)**

Application Load Balancer created to distribute traffic across ECS tasks.

![alt text](<images/load_balancer.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**Target Group Health Status**

Target group showing a healthy EC2 instance registered behind the load balancer.

![alt text](<images/target_group.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**Application Auto Scaling for ECS**

Application AutoScaling configured to manage ECS tasks.

![alt text](<images/service_auto_scaling.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**Route 53 DNS records**

Route 53 DNS record created successfully with Terraform

![alt text](<images/route53_dns_record.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

**CloudWatch logs**

Amazon CloudWatch displaying application server logs

![alt text](<images/cloudwatch_logs.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

## Deployed Products Application
Application base URL:https://products.mike71techsolutions.com 

Application Health: 

![alt text](<images/application_health_status.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------

Application showing products:

![alt text](<images/products_display.png>)
---------------------------------------------------------------------------------------------------------------------------------------------------------------




VPC Endpoints
- This architecture removes NAT Gateway to reduce cost 
by using VPC endpoints for private AWS service communication.