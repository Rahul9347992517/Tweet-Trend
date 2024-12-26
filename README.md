# Valaxy-Project



- Automatically runs tests on pull requests.
- Builds a Docker image and deploys it to a Kubernetes cluster.
- Sends notifications on deployment success or failure.


# Node.js Application CI/CD Pipeline

This project demonstrates a complete CI/CD pipeline for a Node.js application using Jenkins, Docker, Kubernetes, and Helm. The pipeline automates testing, building, deployment, and notification processes.

## Prerequisites

1. **Infrastructure Setup**
   - AWS account for hosting Kubernetes cluster.
   - Jenkins server configured as a CI/CD tool.
   - Terraform installed locally for infrastructure management.
   - Ansible for configuring servers.

2. **Tools and Libraries**
   - Docker
   - Kubectl
   - Helm
   - JFrog Artifactory (optional for artifact management)
   - Node.js installed locally for application development.

3. **Access Configuration**
   - AWS CLI with IAM user credentials for managing cloud resources.
   - GitHub repository for source code hosting.
   - SSH keys added to your GitHub account for secure repository access.

## Pipeline Overview

1. **Source Control**
   - Code is hosted on GitHub.
   - Webhooks trigger Jenkins builds on new commits or pull requests.

2. **Stages in the CI/CD Pipeline**
   - **Test**: Runs unit tests using Mocha/Chai (or any preferred testing framework).
   - **Build**: Packages the application into a Docker image.
   - **Deploy**: Deploys the Docker image to a Kubernetes cluster using Helm.
   - **Notifications**: Sends build and deployment status notifications via email or Slack.

3. **Monitoring and Logging**
   - Prometheus and Grafana for monitoring the Kubernetes cluster.
   - Centralized logging for debugging deployment issues.

## Setup Guide

### 1. Clone Repository

```bash
git clone git@github.com:<your-username>/<your-repo>.git
cd <your-repo>
```

### 2. Configure Jenkins

1. Install necessary plugins:
   - GitHub Integration
   - Pipeline
   - Docker
   - Kubernetes
   - Artifactory

2. Add credentials:
   - GitHub personal access token.
   - AWS CLI credentials for EKS access.
   - Docker credentials for JFrog Artifactory.

3. Create a new Jenkins pipeline job and configure the `Jenkinsfile`:
   - Set SCM to GitHub and link to your repository.
   - Define the pipeline stages in the `Jenkinsfile`.

### 3. Deploy Infrastructure with Terraform

1. Navigate to the `terraform_code` directory:
   ```bash
   cd terraform_code
   ```
2. Initialize Terraform and deploy the infrastructure:
   ```bash
   terraform init
   terraform apply --auto-approve
   ```

### 4. Configure Kubernetes Cluster

1. Update kubeconfig for your EKS cluster:
   ```bash
   aws eks --region <region> update-kubeconfig --name <cluster-name>
   ```
2. Verify cluster access:
   ```bash
   kubectl get nodes
   ```

3. Deploy Helm charts for the Node.js application:
   ```bash
   helm install <release-name> <chart-path>
   ```

### 5. Application Deployment

1. Push the `deploy.sh` and Kubernetes manifest files (`namespace.yaml`, `deployment.yaml`, `service.yaml`) to GitHub.
2. Include the `deploy` stage in the Jenkins pipeline to automate the process.

### 6. Monitoring

1. Deploy Prometheus and Grafana:
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm install monitoring prometheus-community/kube-prometheus-stack
   ```
2. Access Grafana via the LoadBalancer URL and monitor the deployment.

## Cleanup

To destroy the infrastructure:
```bash
cd terraform_code
terraform destroy --auto-approve
```

