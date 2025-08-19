# PolarisCloud NVIDIA DinD Containers

PolarisCloud's customized Docker-in-Docker (DinD) containers with NVIDIA GPU support and SSH access. Available in two variants for different use cases.

## 🚀 Container Variants

### 1. **Base NVIDIA DinD** (`bigideaafrica/nvidia-dind`)

Lightweight container with essential Docker-in-Docker and NVIDIA GPU support.

### 2. **PolarisLLM DinD** (`bigideaafrica/polarisllm-dind`)

Enhanced container with PolarisLLM pre-installed for AI model deployment and management.

## ✨ Features

### Base Container Features

- 🐳 **Docker-in-Docker**: Full Docker engine inside container
- 🚀 **NVIDIA GPU Support**: NVIDIA Container Toolkit integrated
- 🔐 **SSH Access**: Both password and key-based authentication
- 📦 **Multi-Architecture**: AMD64 and ARM64 support
- ⚡ **Automated Builds**: CI/CD pipeline with GitHub Actions
- 🎯 **Kubernetes Ready**: Optimized for K8s deployments

### PolarisLLM Container Additional Features

- 🤖 **PolarisLLM Runtime**: 300+ AI models with OpenAI-compatible APIs
- 🧠 **PyTorch GPU**: CUDA-optimized PyTorch for ML workloads
- 🔌 **API Server**: Built-in API server on port 7860
- 📊 **Model Management**: Deploy, monitor, and manage AI models

## Quick Start

### Base NVIDIA DinD Container

#### Docker Run

```bash
# Run with GPU support
docker run --gpus all -it --privileged bigideaafrica/nvidia-dind:latest

# Run with SSH access (expose port 22)
docker run --gpus all -it --privileged -p 2222:22 bigideaafrica/nvidia-dind:latest

# SSH into the container
ssh root@localhost -p 2222
# Password: nvidia-dind
```

#### Kubernetes Deployment

```bash
# Deploy base container
kubectl apply -f nvidia-dind.yml

# Get SSH service port
kubectl get service nvidia-dind-ssh

# Connect via SSH
ssh root@<node-ip> -p <node-port>
```

### PolarisLLM DinD Container

#### Docker Run (PolarisLLM)

```bash
# Run PolarisLLM container with GPU and API access
docker run --gpus all -it --privileged \
  -p 2222:22 \
  -p 7860:7860 \
  --name polarisllm-dind \
  bigideaafrica/polarisllm-dind:latest

# SSH into the container
ssh root@localhost -p 2222
# Password: nvidia-dind

# Start PolarisLLM server inside container
polarisllm start --daemon

# Deploy an AI model
polarisllm deploy --model qwen2.5-7b-instruct
```

#### Kubernetes Deployment (PolarisLLM)

```bash
# Deploy PolarisLLM container
kubectl apply -f polarisllm-dind.yml

# Get service ports
kubectl get service polarisllm-dind-ssh    # SSH access
kubectl get service polarisllm-dind-api    # PolarisLLM API

# Connect via SSH
ssh root@<node-ip> -p <ssh-port>

# Access PolarisLLM API
curl http://<node-ip>:<api-port>/v1/models
```

#### Using PolarisLLM API

```bash
# Deploy a model inside the container
polarisllm deploy --model qwen2.5-7b-instruct

# Test the API
curl -X POST "http://localhost:7860/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5-7b-instruct",
    "messages": [{"role": "user", "content": "Hello, how are you?"}]
  }'
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

### Base NVIDIA DinD

- `bigideaafrica/nvidia-dind:latest` - Ubuntu 24.04 (recommended)
- `bigideaafrica/nvidia-dind:24.04` - Ubuntu 24.04
- `bigideaafrica/nvidia-dind:22.04` - Ubuntu 22.04  
- `bigideaafrica/nvidia-dind:20.04` - Ubuntu 20.04

### PolarisLLM DinD

- `bigideaafrica/polarisllm-dind:latest` - Ubuntu 24.04 with PolarisLLM (recommended)
- `bigideaafrica/polarisllm-dind:24.04` - Ubuntu 24.04 with PolarisLLM
- `bigideaafrica/polarisllm-dind:22.04` - Ubuntu 22.04 with PolarisLLM
- `bigideaafrica/polarisllm-dind:20.04` - Ubuntu 20.04 with PolarisLLM

## Docker Hub Repositories

- **Base Container**: [bigideaafrica/nvidia-dind](https://hub.docker.com/r/bigideaafrica/nvidia-dind)
- **PolarisLLM Container**: [bigideaafrica/polarisllm-dind](https://hub.docker.com/r/bigideaafrica/polarisllm-dind)
