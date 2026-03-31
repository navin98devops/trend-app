# Trend App — DevOps Deployment Project

## Application
React app deployed to production using Docker, Kubernetes, Jenkins CI/CD.

## Live URLs
- App (EC2): http://13.233.204.44:3000
- App (Kubernetes): http://a216a92cab9d6435c9eadddd958f212e-60301111.ap-south-1.elb.amazonaws.com
- Jenkins: http://13.233.204.44:8080
- Grafana: http://aee76b5d337f946a89d65e35d990d1fc-1645761521.ap-south-1.elb.amazonaws.com
- Prometheus: http://a2005e0308ba14bc3af127aa3c6458f0-1888788300.ap-south-1.elb.amazonaws.com:9090

## Kubernetes LoadBalancer ARN
a216a92cab9d6435c9eadddd958f212e-60301111.ap-south-1.elb.amazonaws.com

## Infrastructure (Created by Terraform)
- VPC with public and private subnets
- EC2 instance (jenkins-server) with Jenkins installed via user_data
- IAM roles and security groups
- EKS cluster (trend-eks-cluster)
- EKS node groups

## Tools Used
- Git + GitHub
- Docker + DockerHub
- AWS EC2 + EKS
- Terraform
- Jenkins
- Kubernetes
- Helm
- Prometheus + Grafana

## Setup Instructions

### 1. Clone the repo
git clone https://github.com/navin98devops/trend-app.git

### 2. Configure AWS credentials
aws configure

### 3. Provision infrastructure with Terraform
cd terraform
terraform init
terraform plan
terraform apply --auto-approve

### 4. Terraform creates automatically:
- VPC, subnets, internet gateway
- EC2 instance with Jenkins installed
- IAM roles and security groups
- EKS cluster with node groups

### 5. Connect to EKS
aws eks update-kubeconfig --region ap-south-1 --name trend-eks-cluster

### 6. Build and run Docker image
docker build -t trend-app .
docker run -p 3000:3000 trend-app

### 7. Deploy to Kubernetes
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

### 8. CI/CD Pipeline
Every push to main branch triggers Jenkins pipeline automatically via GitHub webhook:
- Stage 1: Checkout — pulls latest code from GitHub
- Stage 2: Build Docker Image — builds the image
- Stage 3: Push to DockerHub — pushes image to registry
- Stage 4: Deploy to Kubernetes — applies k8s manifests and restarts deployment

### 9. Monitoring
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
kubectl create namespace monitoring
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring

## Pipeline Stages
1. Checkout — pulls latest code from GitHub
2. Build Docker Image — builds the Docker image
3. Push to DockerHub — pushes image to navin98/trend-app
4. Deploy to Kubernetes — deploys to EKS cluster

## Screenshots
See /screenshots folder for all evidence screenshots.
