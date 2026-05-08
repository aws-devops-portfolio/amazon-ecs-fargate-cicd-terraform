# Amazon ECS Fargate CICD Terraform
Docker image deployment to ECS with Terraform and CICD

## OVERVIEW
This project demonstrates a production-style, multi-environment deployment pipeline for containerized applications on AWS using ECS Fargate, Terraform, Docker, and GitHub Actions.

The solution provisions isolated development and production environments using Infrastructure as Code (Terraform), with automated CI/CD workflows deploying application updates to Amazon ECS Fargate.

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

## Multi-Environment Deployment

The project supports separate `dev` and `prod` environments using environment-specific Terraform backend and variable configurations.

Each environment provisions isolated AWS resources, including:

- VPC and networking components
- ECS Cluster and ECS Service
- ECR repositories
- Application Load Balancer
- CloudWatch monitoring
- Route 53 DNS configuration
- ECS Service Auto Scaling

Environment selection is automated through GitHub Actions branch-based deployments:

| Branch  | Environment |
|---------|-------------|
| develop | dev         |
| main    | prod        |

Terraform state files are separated per environment using dedicated backend configuration files.

## CI/CD Workflow

The GitHub Actions pipeline automates both infrastructure provisioning and application deployment.

Workflow process:

1. Developer pushes code to `develop` or `main`
2. GitHub Actions authenticates to AWS using OIDC
3. Terraform provisions or updates AWS infrastructure
4. Maven builds the Spring Boot application
5. Docker image is built and pushed to Amazon ECR
6. ECS task definition is updated with the new image
7. ECS Service performs rolling deployment on AWS Fargate

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

## Issues experienced
1. OIDC Role Assumption Failure (Github Actions - AWS)
   Issue:
   
   Could not assume role with OIDC: Request ARN is invalid

   Cause:
   
   This can occur if:
   - The ARN for the GitHub role was not correctly formatted, due to account id that was not yet configured in GitHub repository variables.   

   Solution: 

   - Configure the Account ID variable in the GitHub repository variables
  
2. Access Denial issue in Terraform Apply stage
   Issue:
   
   Creating ECR Repository (app_ecr): operation error ECR: CreateRepository, https response error StatusCode: 400, RequestID: 805c2a67-2a90-43fc-b252-2332e6d184da, api error AccessDeniedException: User: arn:aws:sts::############:assumed-role/GitHubAction-Role/GitHub_to_AWS_via_FederatedOIDC is not authorized to perform: ecr:CreateRepository on resource: arn:aws:ecr:us-east-1:############:repository/app_ecr because no identity-based policy allows the ecr:CreateRepository action

   Cause:
   
   - The CreateRepository action has not been explicitly allowed in the GitHubActions role policy permissions. 
   
   Solution: 

   - Update the role policy permissions by adding CreateRepository action and specify "Allow" as effect

3. ECS failure to pull container
   Issue:
   
   CannotPullContainerError: The task cannot pull ############.dkr.ecr.us-east-1.amazonaws.com/product-app-ecr:latest from the registry ############.dkr.ecr.us-east-1.amazonaws.com/product-app-ecr:latest.

   Cause:
   
   - ECS Tasks were not in the same subnets as VPC endpoints. 
   
   Solution: 

   - Update assign_public_ip flag under service network configuration to false
   - Update ECS Task Security group to allow outbound HTTPS

## Future improvements 
   - Add approval gates before production deployment
   - Separate AWS accounts per environment using AWS Organizations

