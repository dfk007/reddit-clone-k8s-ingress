# Technical Documentation: LocalStack with Kubernetes Setup for Reddit Clone

## Overview

This documentation outlines the setup of a local development environment that uses LocalStack to emulate AWS services alongside Kubernetes (via Minikube) for deploying a Reddit clone application.

## Environment Components

### 1. Core Services

- LocalStack (v4.0.3) - AWS service emulator
- Minikube (v1.34.0) - Local Kubernetes cluster
- Docker - Container runtime
- Kubernetes (v1.31.0) - Container orchestration

### 2. Infrastructure Setup

- LocalStack running in Docker mode
- Minikube using Docker driver
- Kubernetes services configured with LoadBalancer type
- Prometheus monitoring stack deployed

## Successful Commands

1. **LocalStack Installation and Setup**

```
pip install localstack
pip install awscli-local
localstack start -d
```

1. **Kubernetes Tools Installation**

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

1. **Minikube Setup**

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
minikube start
```

1. **Docker Image Build**

```
docker build -t reddit-clone .
minikube image load reddit-clone
```

1. **Kubernetes Deployments**

```
kubectl apply -f deployment.yml
kubectl apply -f service.yml
```

1. **LocalStack AWS Service Setup**

```
awslocal s3 mb s3://reddit-clone-assets
awslocal ecr create-repository --repository-name reddit-clone
```

1. **Monitoring Setup**

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack
```

1. **Network Configuration**

```
minikube tunnel
```

## Service Status

The following services were successfully deployed:

- Reddit Clone Application (LoadBalancer service)
- Prometheus Monitoring Stack
- Grafana Dashboard
- Alert Manager
- Node Exporter
- Kube State Metrics

## Verification Commands

```
kubectl get pods
kubectl get services
minikube status
docker ps
```

This setup provides a complete local development environment that mimics a production AWS infrastructure while allowing for local testing and development of the Reddit clone application.