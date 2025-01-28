# GitOps-Based Multi-Account EKS Cluster Management

## Table of Contents
1. [Architecture Overview](#architecture-overview)
2. [Components](#components)
3. [Repository Structure](#repository-structure)
4. [Configuration Files](#configuration-files)
5. [Deployment Flow](#deployment-flow)
6. [Security Considerations](#security-considerations)
7. [Operation Guide](#operation-guide)

## Architecture Overview
This solution implements a GitOps-based approach to managing Amazon EKS clusters across multiple AWS accounts using:
- AWS Controllers for Kubernetes (ACK)
- ArgoCD
- Helm Charts
- Infrastructure as Code (IaC)

### High-Level Architecture
The solution uses a hub-and-spoke model where:
- A central management cluster (hub) orchestrates the creation and management of EKS clusters in different environments
- Spoke clusters are created and managed in separate AWS accounts for dev, staging, and production environments

## Components

### 1. ArgoCD Application Configuration
The `app-of-apps.yaml` implements the ArgoCD app-of-apps pattern to manage multiple EKS cluster deployments. This pattern allows for declarative management of multiple ArgoCD applications.

### 2. Environment-Specific Configurations
Located in `/apps` directory:
- `ekscluster-dev.yaml`
- `ekscluster-stage.yaml`
- `ekscluster-prod.yaml`

These files define environment-specific ArgoCD applications that reference the shared Helm chart with environment-specific values.

### 3. Helm Charts
Located in `/charts/sdlc-clusters/`:
- Main chart configuration in `Chart.yaml`
- Environment-specific values in `values-{env}.yaml`
- Infrastructure templates in `templates/` directory

#### Core Infrastructure Templates:
- `eks-cluster.yaml`: EKS cluster definition
- `vpc.yaml`: VPC configuration
- `subnet1.yaml` & `subnet2.yaml`: Network configuration
- `natgateway.yaml`: NAT Gateway setup
- `nodegroup.yaml`: EKS node group configuration
- Various IAM role configurations (cluster-role.yaml, node-role.yaml, pia-role.yaml)

## Repository Structure
```
├── app-of-apps.yaml              # Root ArgoCD application
├── apps/
│   ├── ekscluster-dev.yaml      # Dev environment configuration
│   ├── ekscluster-stage.yaml    # Staging environment configuration
│   └── ekscluster-prod.yaml     # Production environment configuration
└── charts/
    └── sdlc-clusters/
        ├── Chart.yaml           # Helm chart metadata
        ├── values-dev.yaml      # Dev environment values
        ├── values-stage.yaml    # Staging environment values
        ├── values-prod.yaml     # Production environment values
        └── templates/           # Infrastructure templates
            ├── eks-cluster.yaml
            ├── vpc.yaml
            ├── subnet1.yaml
            └── ...
```

## Configuration Files

### Environment Values
Each environment (dev, staging, prod) has its own values file (`values-{env}.yaml`) that specifies:
- Namespace configuration
- Cluster specifications
- Node group configurations
- Network settings
- Tags and labels

### Infrastructure Templates
The templates directory contains Kubernetes manifests that use ACK controllers to provision AWS resources:
1. **Network Infrastructure**
   - VPC configuration
   - Subnet definitions
   - Internet Gateway
   - NAT Gateway
   - Route Tables

2. **EKS Resources**
   - Cluster configuration
   - Node groups
   - IAM roles and policies

## Deployment Flow

1. **Initial Setup**
   - ACK controllers are installed in the management cluster
   - Required IAM roles and permissions are configured
   - ArgoCD is installed and configured

2. **Cluster Provisioning**
   - ArgoCD monitors the Git repository
   - Changes to configuration trigger automated deployments
   - ACK controllers create/update AWS resources
   - EKS clusters are provisioned with specified configurations

3. **Ongoing Management**
   - Infrastructure changes are made through Git
   - ArgoCD ensures desired state matches actual state
   - Version control provides audit trail and rollback capability

## Security Considerations

### IAM Configuration
- Separate IAM roles for cluster and nodes
- Principle of least privilege applied
- Cross-account role assumptions configured

### Network Security
- VPC isolation between environments
- Private subnets for worker nodes
- Controlled ingress/egress through NAT Gateway

## Operation Guide

### Adding a New Environment
1. Create new values file (`values-{env}.yaml`)
2. Add corresponding ArgoCD application in `/apps`
3. Configure necessary AWS account access
4. Apply changes through Git

### Updating Existing Clusters
1. Modify relevant values file
2. Commit changes to Git repository
3. ArgoCD will automatically sync changes

### Monitoring and Troubleshooting
1. Use ArgoCD UI to monitor sync status
2. Check ACK controller logs for AWS resource issues
3. Review EKS cluster CloudWatch logs
4. Monitor AWS CloudTrail for API activity

For detailed implementation steps and example configurations, refer to the README.md file in the repository root.