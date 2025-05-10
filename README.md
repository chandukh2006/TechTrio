# DeepSeek 7B with Ollama and Open WebUI - Windows Setup Guide

## Project Overview

This project enables privacy-focused AI applications by deploying DeepSeek 7B locally, minimizing digital footprints while still providing powerful language model capabilities.

### Applications & Use Cases

- **Privacy-Preserving AI**: Process sensitive data locally without external API calls
- **Offline AI Assistance**: Access AI capabilities without internet connectivity
- **Reduced Data Footprint**: Minimize tracking and data storage concerns
- **Customized AI Solutions**: Fine-tune models for specific domain applications
- **Educational Purposes**: Learn about AI models in a controlled environment
- **API Integration**: Build applications that leverage local AI capabilities
- **Cost-Effective Deployment**: Eliminate ongoing API usage fees

## Prerequisites

- Windows 10/11 (64-bit)
- Administrative privileges
- At least 16GB RAM recommended
- 30GB+ free disk space
- NVIDIA GPU with updated drivers (optional but recommended)

## Installation Steps

### 1. Install Docker Desktop for Windows

```powershell
# Download and install Docker Desktop from https://www.docker.com/products/docker-desktop/
```

### 2. Run Ollama Container

```powershell
# Create a Docker volume for Ollama data
docker volume create ollama

# Run Ollama container
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

### 3. Test Ollama API

```powershell
# Verify Ollama API is working
Invoke-RestMethod -Uri "http://localhost:11434"
```

### 4. Pull the DeepSeek Model

```powershell
# Pull the DeepSeek model
docker exec -it ollama ollama pull deepseek-r1:7b-qwen-distill-q4_K_M
```

### 5. Deploy Open WebUI

```powershell
# Run the Open WebUI container
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v ollama-webui:/app/backend/data --name ollama-webui ghcr.io/ollama-webui/ollama-webui:main
```

### 6. Set up Nginx for Reverse Proxy (Optional)

```powershell
# Install Chocolatey if needed
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install Nginx
choco install nginx -y

# Configure Nginx (save this to C:\tools\nginx\conf\nginx.conf)
# http {
#    server {
#        listen 80;
#        server_name localhost;
#
#        location / {
#            proxy_pass http://localhost:3000;
#            proxy_set_header Host $host;
#            proxy_set_header X-Real-IP $remote_addr;
#            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
#            proxy_set_header X-Forwarded-Proto $scheme;
#        }
#    }
# }

# Start Nginx
cd C:\tools\nginx
.\nginx.exe

# To reload after config changes
.\nginx.exe -s reload
```

## Usage

Access the Open WebUI by navigating to `http://localhost:3000` in your web browser.
