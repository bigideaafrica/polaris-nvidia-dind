# PolarisCloud NVIDIA DinD Container

PolarisCloud's customized Docker-in-Docker (DinD) container with NVIDIA GPU support and SSH access. Built for developing, testing, and deploying Docker containers with NVIDIA GPUs in cloud environments.

## ✨ Features

- 🐳 **Docker-in-Docker**: Full Docker engine inside container
- 🚀 **NVIDIA GPU Support**: NVIDIA Container Toolkit integrated
- 🔐 **SSH Access**: Both password and key-based authentication
- 📦 **Multi-Architecture**: AMD64 and ARM64 support
- ⚡ **Automated Builds**: CI/CD pipeline with GitHub Actions
- 🎯 **Kubernetes Ready**: Optimized for K8s deployments

## Quick Start

### Docker Run

```bash
# Run with GPU support
docker run --gpus all -it --privileged bigideaafrica/nvidia-dind:latest

# Run with SSH access (expose port 22)
docker run --gpus all -it --privileged -p 2222:22 bigideaafrica/nvidia-dind:latest

# SSH into the container
ssh root@localhost -p 2222
# Password: nvidia-dind
```

### Kubernetes Deployment

```bash
# Deploy container
kubectl apply -f nvidia-dind.yml

# Get SSH service port
kubectl get service nvidia-dind-ssh

# Connect via SSH
ssh root@<node-ip> -p <node-port>
```

## Environment Variables

- `DOCKERD_FLAG`: Append command-line flags to `dockerd`

## Requirements

- Host with NVIDIA Container Toolkit installed
- Privileged mode or [Sysbox](https://github.com/nestybox/sysbox) with NVIDIA GPU access
- For Kubernetes: Node with NVIDIA GPU support

## SSH Authentication

The container supports both authentication methods:

- **Password**: `nvidia-dind` (default root password)
- **SSH Keys**: Add your public keys to `/root/.ssh/authorized_keys`

## Available Tags

- `bigideaafrica/nvidia-dind:latest` - Ubuntu 24.04 (recommended)
- `bigideaafrica/nvidia-dind:24.04` - Ubuntu 24.04
- `bigideaafrica/nvidia-dind:22.04` - Ubuntu 22.04  
- `bigideaafrica/nvidia-dind:20.04` - Ubuntu 20.04

## Docker Hub Repository

[bigideaafrica/nvidia-dind](https://hub.docker.com/r/bigideaafrica/nvidia-dind)
