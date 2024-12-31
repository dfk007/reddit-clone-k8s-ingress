# [Reddit Clone with Kubernetes and LocalStack](Readme.md) | [Detailed-Overview](docs/documentation.md) | [Low-Level-Setup](docs/technical-setup-LowLevel.md)

This project demonstrates a Reddit clone application deployed on Kubernetes with LocalStack for local AWS service emulation.

## Project Setup

### Prerequisites Installed

- LocalStack
- AWS CLI Local
- Kubernetes (kubectl)
- Minikube
- Helm
- Docker

### Current Configuration

- LocalStack running with AWS services emulated locally
- Minikube cluster running with:
    - Reddit clone deployment (2 replicas)
    - LoadBalancer service exposing port 3000
    - Prometheus monitoring stack installed via Helm

### AWS Resources Created (LocalStack)

- S3 bucket: `reddit-clone-assets`
- ECR repository: `reddit-clone`

### Kubernetes Resources

- Deployment: `reddit-clone-deployment`
- Service: `reddit-clone-service` (LoadBalancer)
- Prometheus monitoring stack

### Monitoring

- Prometheus and Grafana installed via Helm
- Node exporter and metrics server configured
- Kubernetes state metrics enabled

## Access Points

- Reddit Clone App: http://localhost:3000
- Grafana Dashboard: Available through Kubernetes service
- Prometheus: Available through Kubernetes service

## Development Notes

- Docker image built and loaded into Minikube
- Minikube tunnel running for LoadBalancer access
- LocalStack services accessible on localhost:4566