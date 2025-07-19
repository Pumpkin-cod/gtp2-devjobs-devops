# DevJobs Full Stack Platform - DevOps Documentation

A scalable, containerized job portal application deployed on AWS using modern DevOps practices.

This documentation provides an overview of how DevOps enables seamless CI/CD, container orchestration, and infrastructure automation using Terraform, GitHub Actions, Docker, Amazon ECS, and integrations with Slack and SNS.

## Architecture Overview

| Layer | Stack |
|-------|-------|
| Frontend | Angular 19, Nginx, Docker |
| Backend | Spring Boot, Docker |
| CI/CD | GitHub Actions |
| IaC | Terraform |
| Hosting | AWS ECS Fargate, ECR, ALB |
| Notification | Slack + Amazon SNS |

## Live URLs

- **Frontend:** http://gtp2-fe-testing-alb-512036373.eu-central-1.elb.amazonaws.com
- **Backend:** http://gtp2-be-testing-alb-114080375.eu-central-1.elb.amazonaws.com

## DevOps Implementation Details

### Infrastructure as Code (Terraform)

Used Terraform to provision:

- VPCs, subnets, and security groups
- ECS clusters & services (for frontend and backend)
- ALBs with listener rules
- ECR repositories for image storage
- IAM roles and policies
- SNS topics for notification

All infrastructure is version-controlled, reproducible, and deployed consistently across environments.

### Dockerized Workflows

Multi-stage builds were used for optimized image sizes:

- Angular built in Node container, served via Nginx
- Spring Boot is built in a Maven container, deployed using a lightweight JRE
- The .dockerignore is used to reduce image bloat
- All containers run as non-root users for security

### CI/CD Pipeline (GitHub Actions)

A self-hosted GitHub Actions runner:

- Builds and pushes Docker images to Amazon ECR
- Triggers ECS rolling deployments with aws ecs update-service
- Runs automated tests (npm test, mvn test) before deploy
- Sends deployment status notifications to Slack

Sample steps:
1. Checkout code
2. Install dependencies
3. Run tests
4. Build Docker image
5. Push to ECR
6. Deploy to ECS
7. Notify via Slack

### Load Balancing with ALB

- Frontend: Served via AWS Application Load Balancer (HTTP, port 80)
- Backend: Exposed through secure ALB (HTTPS, port 443)
- ALB health checks are used for container readiness
- ECS automatically restarts failed tasks

### Slack & SNS Notifications

- Deployment status updates are sent to Slack channels
- AWS SNS is integrated for alerts and backup notifications
- Slack Webhooks are configured via GitHub Actions secrets

## Testing Strategy

| Layer | Tool | Scope |
|-------|------|-------|
| Frontend | npm test | Unit/component testing |
| Backend | JUnit | Unit and integration |
| CI/CD | GitHub | Status checks, health |


## Infrastructure Documentation

Detailed documentation for the infrastructure is available in the following links:

- **For Product Owners/Clients:** [Client Infrastructure Documentation](https://github.com/AmaliTech-Training-Academy/gtp2-devjobs-backend/blob/develop/infrastructure/README-CLIENT.md)
- **For Developers:** [Technical Infrastructure Documentation](https://github.com/AmaliTech-Training-Academy/gtp2-devjobs-backend/blob/develop/infrastructure/README.md)


## Security Best Practices

- No secrets committed to source control
- Docker containers run as non-root
- IAM roles follow the least privilege principle
- GitHub secrets used for AWS credentials

