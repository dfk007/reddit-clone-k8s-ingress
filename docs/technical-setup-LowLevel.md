# Technical Setup Documentation

## Environment Details

### System Configuration
- OS: Linux (Arch Linux)
- Docker Version: 27.2.0
- Kubernetes Version: v1.31.0
- LocalStack Version: 4.0.3
- Minikube Version: v1.34.0

## Successful Command Execution Log

### 1. LocalStack Setup
```bash
# Install LocalStack and AWS CLI Local
pip install localstack  # Successfully installed localstack-4.0.3
pip install awscli-local  # Successfully installed awscli-local-0.22.0

# Start LocalStack in detached mode
localstack start -d  # Successfully started LocalStack container
```

### 2. Kubernetes Tools Installation
```bash
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
```

### 3. Container and Cluster Operations
```bash
# Start Minikube cluster
minikube start  # Successfully started using Docker driver

# Build and Load Docker Image
docker build -t reddit-clone .  # Successfully built image
minikube image load reddit-clone  # Successfully loaded into Minikube

# Verify Docker Status
docker ps  # Shows running containers:
# - localstack/localstack-pro
# - gcr.io/k8s-minikube/kicbase
```

### 4. AWS Resource Creation (LocalStack)
```bash
# Create S3 bucket
awslocal s3 mb s3://reddit-clone-assets  # Successfully created bucket

# Create ECR repository
awslocal ecr create-repository --repository-name reddit-clone
# Successfully created with URI: 000000000000.dkr.ecr.us-east-1.localhost.localstack.cloud:4566/reddit-clone
```

### 5. Kubernetes Deployments
```bash
# Apply Kubernetes configurations
kubectl apply -f deployment.yml  # Successfully created deployment
kubectl apply -f service.yml  # Successfully created service

# Start Minikube tunnel for LoadBalancer access
nohup minikube tunnel > minikube-tunnel.log 2>&1 &  # Successfully running
```

### 6. Monitoring Stack Setup
```bash
# Install Metrics Server
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

# Setup Prometheus Stack
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack
```

## Verification Commands and Results

### Pod Status
```bash
kubectl get pods
# Results:
# - prometheus-kube-prometheus-operator: Running
# - reddit-clone-deployment pods (2): Running
# - prometheus-node-exporter: Running
# - prometheus-grafana: Running
```

### Service Status
```bash
kubectl get services
# Results:
# - reddit-clone-service: LoadBalancer
# - prometheus services: ClusterIP
# - kubernetes: ClusterIP
```

## Port Mappings
- LocalStack: 4566 (main interface)
- Reddit Clone: 3000 (via LoadBalancer)
- Prometheus: 9090
- Grafana: 80

## Known Working State
- LocalStack container: Healthy and running
- Minikube cluster: Running
- Kubernetes services: All operational
- Monitoring stack: Deployed and functional
- LoadBalancer: Accessible via Minikube tunnel

## Troubleshooting Notes
- If LoadBalancer services are not accessible, verify Minikube tunnel is running
- For image pull issues, ensure images are properly loaded into Minikube
- Monitor resource usage with `kubectl top nodes` and `kubectl top pods`