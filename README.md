# Trend App — DevOps Deployment Project

## Application
React app deployed to production using Docker, Kubernetes, Jenkins CI/CD.

## Live URLs
- App: http://a512cc34a911d4600a0ef7ee476e378b-1809705446.ap-south-1.elb.amazonaws.com
- Jenkins: http://15.207.113.190:8080
- Grafana: http://a040a22f2552d46beafba9a1563de7a5-1162104737.ap-south-1.elb.amazonaws.com
- Prometheus: http://a4619368a1c59400ca639d8c6f5dddbe-721573914.ap-south-1.elb.amazonaws.com:9090

## LoadBalancer ARN
a512cc34a911d4600a0ef7ee476e378b-1809705446.ap-south-1.elb.amazonaws.com

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

### 2. Build Docker image
docker build -t trend-app .
docker run -p 3000:3000 trend-app

### 3. Provision infrastructure
cd terraform-eks
terraform init
terraform apply --auto-approve

### 4. Connect to EKS
aws eks update-kubeconfig --region ap-south-1 --name trend-eks-cluster

### 5. Deploy to Kubernetes
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

### 6. CI/CD Pipeline
Every push to main branch triggers Jenkins pipeline:
- Checkout code from GitHub
- Build Docker image
- Push to DockerHub
- Deploy to EKS

### 7. Monitoring
kubectl create namespace monitoring
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --set grafana.service.type=LoadBalancer

## Pipeline Stages
1. Checkout — pulls latest code from GitHub
2. Build Docker Image — builds the image
3. Push to DockerHub — pushes image to registry
4. Deploy to Kubernetes — applies k8s manifests and restarts deployment

## Screenshots
See /screenshots folder for all evidence screenshots.
