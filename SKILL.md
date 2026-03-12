name: prime-crafter
version: 1.0.0
description: AI-powered infrastructure automation tool for crafting, deploying, and managing cloud-native infrastructure as code using AI-driven templates and best practices.
author: OpenClaw Team
tags: infrastructure, ai, automation, cloud-native, IaC
dependencies:
  - terraform >= 1.5.0
  - ansible >= 2.15.0
  - kubectl >= 1.28.0
  - docker >= 24.0.0
  - python >= 3.9.0
  - awscli >= 2.13.0
  - az >= 2.50.0
  - gcloud >= 450.0.0
environment_variables:
  PRIME_CRAFTER_API_KEY: Required API key for AI model access
  PRIME_CRAFTER_MODEL: Optional, defaults to gpt-4o; supports gpt-3.5-turbo, claude-3-sonnet
  PRIME_CRAFTER_REGION: Optional, defaults to us-east-1 for AWS deployments
  PRIME_CRAFTER_LOG_LEVEL: Optional, defaults to INFO; options: DEBUG, INFO, WARN, ERROR
requirements:
  - Active cloud provider account (AWS, Azure, GCP) with programmatic access
  - Sufficient IAM permissions for infrastructure provisioning
  - Network connectivity to cloud APIs
  - At least 8GB RAM for AI processing
  - Supported OS: Linux, macOS, Windows (WSL2)
---

# Prime Crafter Skill

## Purpose

Prime Crafter is an AI-powered infrastructure automation skill designed to generate, validate, and deploy production-ready infrastructure as code (IaC) templates for cloud-native applications. It leverages advanced AI models to craft Terraform modules, Ansible playbooks, Kubernetes manifests, and Docker configurations based on user-specified requirements, ensuring security, scalability, and best practices compliance.

### Real Use Cases

1. **Rapid Prototype Deployment**: Generate a complete VPC setup with EC2 instances, RDS databases, and load balancers for a web application in minutes, reducing manual scripting time from hours to seconds.
2. **Multi-Cloud Migration**: Automate the creation of equivalent infrastructure resources across AWS, Azure, and GCP, including network peering, security groups, and CI/CD pipelines for zero-downtime migrations.
3. **Compliance-Driven IaC**: Craft HIPAA-compliant healthcare data pipelines with encrypted storage, audit logging, and automated backups, ensuring regulatory adherence without manual policy configuration.
4. **Microservices Orchestration**: Generate Kubernetes deployments with Istio service mesh, Prometheus monitoring, and Jaeger tracing for a containerized e-commerce platform, optimizing for high availability and auto-scaling.
5. **Disaster Recovery Setup**: Create geo-redundant infrastructure with Route 53 failover routing, DynamoDB global tables, and Lambda-based backup automation for financial trading systems.

## Scope

Prime Crafter operates within the infrastructure domain, focusing on IaC generation and deployment. It does not handle application code, database schema design, or frontend development.

### Specific Commands

- `prime-crafter generate --template vpc --provider aws --region us-west-2 --security-level high --output ./infrastructure/vpc.tf`: Generates a VPC template with high-security configuration.
- `prime-crafter validate --input ./infrastructure/ --provider azure --check compliance --standard soc2`: Validates IaC against SOC2 compliance standards.
- `prime-crafter deploy --input ./infrastructure/ --provider gcp --dry-run --auto-approve`: Performs a dry-run deployment to Google Cloud.
- `prime-crafter monitor --resource eks-cluster --provider aws --metrics cpu,memory --alert-threshold 80`: Monitors EKS cluster resources with alerting.
- `prime-crafter optimize --input ./infrastructure/ --cost-target 1000 --region eu-central-1`: Optimizes infrastructure for cost efficiency targeting $1000/month in EU Central.
- `prime-crafter audit --input ./infrastructure/ --provider multi --report json`: Generates a multi-cloud security audit report in JSON format.

## Work Process

Prime Crafter follows a structured, AI-guided workflow to ensure reliable infrastructure creation:

1. **Requirement Analysis**: Parse user input for cloud provider, resource types, security requirements, and scaling needs using natural language processing.
2. **Template Selection**: Match requirements to pre-trained templates (e.g., ECS Fargate for serverless, EKS for Kubernetes) with AI recommendations.
3. **Code Generation**: Use AI to generate IaC code, incorporating best practices like tagging, encryption, and least-privilege IAM.
4. **Validation Phase**: Run automated checks for syntax errors, security vulnerabilities (via tools like Checkov), and cost estimates.
5. **Simulation**: Execute dry-run deployments in a sandboxed environment to predict resource usage and potential conflicts.
6. **Deployment**: Apply changes with auto-approval for non-critical resources, requiring manual confirmation for production environments.
7. **Monitoring Integration**: Embed monitoring dashboards and alerts using CloudWatch, Cloud Monitoring, or Azure Monitor.
8. **Documentation**: Auto-generate README files, architecture diagrams, and runbooks for the created infrastructure.

### Verification Steps

- **Syntax Check**: Run `terraform validate` or equivalent to ensure no parsing errors.
- **Security Scan**: Execute `checkov -f ./infrastructure/` to identify vulnerabilities.
- **Cost Estimation**: Use `infracost breakdown --path ./infrastructure/` to verify budget compliance.
- **Dry-Run Success**: Confirm `terraform plan` shows no destructive changes without `--auto-approve`.
- **Resource Health**: Post-deployment, query APIs (e.g., `aws ec2 describe-instances`) to confirm instances are running.

## Golden Rules

1. **Never Deploy to Production Without Review**: Always require human approval for prod deployments; use `--dry-run` for initial tests.
2. **Encrypt Secrets**: Automatically generate and use AWS KMS, Azure Key Vault, or GCP Secret Manager for all sensitive data.
3. **Follow Least Privilege**: AI-generated IAM roles include only necessary permissions; avoid wildcard policies.
4. **Version Control IaC**: Commit all generated code to Git with descriptive messages; tag releases.
5. **Monitor Costs**: Integrate cost alerts and set budgets; optimize automatically if thresholds are exceeded.
6. **Backup Critical Data**: Include automated snapshots and cross-region replication for stateful resources.
7. **Test Failover**: Generate chaos engineering scripts to simulate failures and validate recovery.
8. **Document Dependencies**: List all external services, APIs, and network requirements in generated READMEs.

## Examples

### Example 1: Generate a Secure VPC for a Web App

**Input Prompt**: "Create a VPC with public and private subnets, NAT gateway, security groups for HTTP/HTTPS, and an RDS PostgreSQL instance for my e-commerce site in AWS us-east-1."

**Generated Output**:
```hcl
# vpc.tf
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "ecommerce-vpc"
    Environment = "production"
  }
}

resource "aws_subnet" "public" {
  count = 2
  vpc_id = aws_vpc.main.id
  cidr_block = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index)
  availability_zone = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true
}

# Additional resources for NAT, security groups, and RDS...
```

**CLI Execution**: `prime-crafter generate --prompt "Create a VPC with public and private subnets, NAT gateway, security groups for HTTP/HTTPS, and an RDS PostgreSQL instance for my e-commerce site in AWS us-east-1." --output ./infrastructure/`

### Example 2: Deploy a Kubernetes Cluster with Monitoring

**Input Prompt**: "Deploy an EKS cluster with 3 nodes, Prometheus monitoring, and Grafana dashboard in AWS."

**Generated Output**:
- Terraform files for EKS setup
- Helm charts for Prometheus/Grafana
- Kustomize overlays for customization

**CLI Execution**: `prime-crafter deploy --input ./k8s-cluster/ --provider aws --monitor --dashboard grafana`

### Example 3: Optimize Existing Infrastructure for Cost

**Input Prompt**: "Reduce costs for my Azure VM farm by 30% without downtime."

**Generated Output**: Migration to spot instances, reserved instances recommendations, and autoscaling policies.

**CLI Execution**: `prime-crafter optimize --input ./azure-vms/ --cost-reduction 30 --no-downtime`

## Rollback Commands

- `prime-crafter rollback --deployment-id 12345 --provider aws --confirm`: Reverts the last deployment to AWS, destroying created resources.
- `prime-crafter undo --input ./infrastructure/ --version v1.2.3 --provider gcp`: Reverts to a previous IaC version in GCP.
- `prime-crafter destroy --resource rds-instance --provider azure --backup-first`: Destroys an RDS instance after creating a backup in Azure.
- `prime-crafter revert-changes --dry-run --input ./infrastructure/`: Shows what would be reverted without applying.

### Troubleshooting Common Issues

1. **API Rate Limits**: If deployments fail due to rate limits, use `--retry-delay 30` to add exponential backoff.
2. **Dependency Conflicts**: Run `prime-crafter validate --check dependencies` to identify missing providers or modules.
3. **Security Violations**: Address Checkov errors by regenerating with `--security-level strict`.
4. **Cost Overruns**: Enable `--cost-alert` to pause deployments exceeding budgets.
5. **Network Timeouts**: Increase `--timeout 600` for large deployments in constrained networks.