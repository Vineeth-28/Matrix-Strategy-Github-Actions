# Matrix Strategy Docker Deployment

A Python application demonstrating **GitHub Actions Matrix Strategy**, **Docker containerization**, and **self-hosted runner deployment**.

## Overview

This project showcases a complete CI/CD pipeline with:
- ✅ **Matrix Strategy Testing** - Test Python app across multiple Python versions (3.9, 3.10, 3.11)
- 🐳 **Docker Containerization** - Build and push Docker images to Docker Hub
- 🚀 **Self-Hosted Runner Deployment** - Deploy containerized app using GitHub Actions self-hosted runner

## Project Structure

```
.
├── app.py                      # Simple Python application
├── requirements.txt            # Python dependencies
├── Dockerfile                  # Docker image definition
├── docker-compose.yml          # Docker Compose configuration
└── .github/workflows/          # GitHub Actions workflows
    ├── matrix.yml              # Matrix strategy testing
    ├── Docker.yml              # Docker build & push
    └── deploy.yml              # Self-hosted runner deployment
```

## Prerequisites

- GitHub repository with Actions enabled
- Docker Hub account
- Self-hosted runner configured (for deployment workflow)

## Setup Instructions

### 1. Configure GitHub Secrets & Variables

Store your credentials securely in GitHub:

**Settings → Secrets and variables → Actions**

- `DOCKERHUB_TOKEN` - Docker Hub personal access token
- `DOCKERHUB_USERNAME` - Docker Hub username (as variable)

### 2. Set Up Self-Hosted Runner (Optional)

To enable the `deploy.yml` workflow on your own machine:

```bash
# Follow GitHub's runner setup guide in:
# Settings → Actions → Runners → New self-hosted runner

# Quick start:
mkdir -p ~/actions-runner && cd ~/actions-runner
curl -o actions-runner-linux-x64-*.tar.gz -L https://github.com/actions/runner/releases/download/v2.x.x/actions-runner-linux-x64-*.tar.gz
tar xzf ./actions-runner-linux-x64-*.tar.gz
./config.sh --url <YOUR_REPO_URL> --token <GITHUB_TOKEN>
./run.sh
```

## GitHub Actions Workflows

### 1. Matrix Strategy Testing (`matrix.yml`)

Automatically tests the Python application across multiple Python versions on every push to `main`.

**Triggered by:**
- Push to `main` branch
- Manual workflow dispatch

**Matrix Configuration:**
- Python 3.9, 3.10, 3.11

**Steps:**
1. Checkout code
2. Setup Python (version from matrix)
3. Display Python version
4. Install dependencies
5. Run Python application

```yaml
strategy:
  matrix:
    python-version: ["3.9", "3.10", "3.11"]
```

### 2. Docker Build & Push (`Docker.yml`)

Builds Docker image and pushes to Docker Hub.

**Triggered by:**
- Push to `main` branch
- Manual workflow dispatch

**Steps:**
1. Checkout code
2. Login to Docker Hub
3. Setup Docker Buildx
4. Build and push image as `vineet2804/myapp:latest`

### 3. Self-Hosted Runner Deployment (`deploy.yml`)

Deploys the application on a self-hosted runner using Docker Compose.

**Triggered by:**
- Manual workflow dispatch

**Steps:**
1. Checkout code
2. Start services with Docker Compose
3. Verify running containers

```bash
docker compose up -d
docker ps
```

## Local Development

### Run Python App Directly

```bash
pip install -r requirements.txt
python app.py
```

### Run with Docker

**Build image:**
```bash
docker build -t myapp:latest .
```

**Run container:**
```bash
docker run myapp:latest
```

### Run with Docker Compose

```bash
docker compose up -d
docker compose logs
docker compose down
```

## Workflow Triggers

### Manual Trigger
All workflows support manual dispatch via GitHub Actions UI:
- Go to **Actions** → Select workflow → **Run workflow**

### Automatic Trigger
- `matrix.yml` and `Docker.yml` run automatically on push to `main`
- `deploy.yml` requires manual trigger

## Environment & Configuration

| File | Purpose |
|------|---------|
| `app.py` | Main Python application |
| `requirements.txt` | Python package dependencies |
| `Dockerfile` | Container image definition |
| `docker-compose.yml` | Multi-container orchestration |

**Docker Image Details:**
- Base: `python:3.13-slim`
- Registry: Docker Hub
- Repository: `vineet2804/myapp:latest`
- Container Name: `my-python-app`

## Troubleshooting

### Docker Hub Login Fails
- Verify `DOCKERHUB_TOKEN` is set in GitHub Secrets
- Generate new token: https://hub.docker.com/settings/security

### Self-Hosted Runner Not Available
- Ensure runner is registered: Settings → Actions → Runners
- Verify runner status is "Idle"
- Check runner logs in the runner directory

### Docker Compose Service Fails
- Ensure Docker daemon is running on self-hosted runner
- Check image exists in Docker Hub: `docker pull vineet2804/myapp:latest`

## Key Features

🔄 **Matrix Strategy**
- Test across multiple Python versions simultaneously
- Identify compatibility issues early

🐳 **Docker & Registry**
- Containerized deployment for consistency
- Automated builds on every push
- Centralized image storage in Docker Hub

⚙️ **Self-Hosted Runner**
- Deploy on your own infrastructure
- Full control over deployment environment
- Seamless integration with GitHub Actions

## Author

Created as a demonstration of GitHub Actions best practices and CI/CD workflows.

## License

MIT
