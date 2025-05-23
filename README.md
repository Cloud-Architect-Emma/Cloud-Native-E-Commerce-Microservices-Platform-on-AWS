# Cloud-Native E-Commerce Microservices Platform on AWS

This project demonstrates a cloud-native, microservices-based e-commerce platform leveraging a hybrid architecture using AWS serverless and containerized services. It is designed for scalability, security, and modularity across multiple environments.

## Architecture Overview

This solution follows a layered architecture, integrating frontend, authentication, backend services, data persistence, and monitoring using fully managed AWS services.

Frontend (React)
→ Amazon S3 (Static Website Hosting)
→ API Gateway (REST API Endpoints)
→ AWS Lambda (JWT-based Auth Layer)
→ ECS Fargate (Product, User, Cart Microservices)
→ DynamoDB (NoSQL Database)
→ CloudWatch (Logging & Monitoring)

## Components

### 1. Frontend (React + S3 Static Website Hosting)

- Built as a Single Page Application (SPA) using React.
- Deployed to Amazon S3 with public read access for static website hosting.
- Optionally served over HTTPS via Amazon CloudFront.

### 2. API Gateway + AWS Lambda (JWT Auth Layer)

- API Gateway exposes RESTful endpoints to the frontend.
- A Lambda function acts as a custom authorizer, validating JWTs and enforcing access control.
- Authenticated requests are routed to downstream ECS microservices.

### 3. ECS Fargate (Product, User, Cart Microservices)

- Each microservice is deployed independently in ECS using Fargate launch type.
- Containers are stateless and interact with DynamoDB.
- Services:
  - **Product Service**: Catalog and product details.
  - **User Service**: User registration, login, and profile.
  - **Cart Service**: Shopping cart management.

### 4. DynamoDB

- A NoSQL database storing domain-specific data for each microservice.
- Uses composite keys and GSIs for optimized access patterns.
- Benefits include seamless scaling, high availability, and cost efficiency.

### 5. CloudWatch

- Centralized logging and monitoring solution.
- Captures logs from Lambda and ECS services.
- Provides dashboards, metrics, and alarms for operational insight.

## Deployment Steps

### Step 1: Frontend (React → S3)

```bash
npm run build
aws s3 sync ./build s3://your-bucket-name --acl public-read
Enable static website hosting in S3 bucket properties.
```

## Step 2: Lambda Authorizer
Deploy authHandler.js using AWS CLI or Console.

Configure it as a Lambda Authorizer for API Gateway.

## Step 3: API Gateway
Define REST routes.

Attach Lambda Authorizer.

Configure integration with ECS Fargate endpoints via ALB or VPC Link.

## Step 4: ECS Fargate
Build and push Docker images to Amazon ECR.

Create ECS Task Definitions for each microservice.

Launch Services using Fargate with desired CPU/memory settings.

Register services with an Application Load Balancer (if required).

## Step 5: DynamoDB
Create tables using AWS Console, CloudFormation, or Terraform.

Use on-demand mode for auto-scaling.

## Step 6: Monitoring with CloudWatch
Ensure ECS Task Definitions include CloudWatch log configuration.

Set up custom metrics and alarms.

## Project Directory Structure

├── frontend/                # React frontend
├── auth/                   # Lambda JWT authorizer
├── services/
│   ├── product-service/    # Product microservice
│   ├── user-service/       # User microservice
│   └── cart-service/       # Cart microservice
├── infrastructure/         # IaC scripts (Terraform/CloudFormation)
└── README.md

## AWS Services Used

AWS Service	Role in Architecture
S3	Static website hosting
API Gateway	REST API layer
AWS Lambda	JWT-based custom authorizer
ECS Fargate	Containerized microservices
DynamoDB	Serverless NoSQL database
CloudWatch	Centralized logging and monitoring
ECR	Docker image registry

## Security Best Practices

Use IAM roles with least privilege.

Enable logging for API Gateway and ECS.

Secure S3 bucket with public access block (use CloudFront for secure delivery).

Enforce JWT verification at Lambda and service levels.

## Future Enhancements
CI/CD pipeline with GitHub Actions and AWS CodePipeline.

HTTPS frontend via CloudFront + ACM.

Automated infrastructure provisioning via Terraform modules.

Integration with Cognito for managed user pools.


## License
MIT License © Emma.


