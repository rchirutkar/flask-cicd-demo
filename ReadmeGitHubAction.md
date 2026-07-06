# Flask CI/CD Pipeline using GitHub Actions

## Project Overview

This project demonstrates a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Flask web application using **GitHub Actions**. The pipeline automates dependency installation, testing, build validation, and deployment to an AWS EC2 instance.

The project also demonstrates the use of GitHub Environments, Repository Secrets, Environment Secrets, manual approval for production deployments, and secure SSH-based deployment.

---

## Objectives

- Automate application build and testing.
- Deploy automatically to a staging environment.
- Deploy to production only after creating a GitHub Release.
- Secure sensitive information using GitHub Secrets.
- Implement Environment-based deployment approvals.
- Perform automated health checks after deployment.

---

## Technology Stack

| Technology | Version |
|------------|---------|
| Python | 3.12 |
| Flask | Latest |
| GitHub Actions | CI/CD |
| Git | Latest |
| Ubuntu | 24.04 LTS |
| AWS EC2 | Ubuntu Instance |
| MongoDB Atlas | Cloud Database |
| Pytest | Unit Testing |
| OpenSSH | Remote Deployment |

---

## Repository Structure

```
flask-cicd-demo
│
├── .github
│   └── workflows
│       └── ci-cd.yml
│
├── tests/
│
├── app.py
├── requirements.txt
├── READMEGitHubAction.md
└── ...
```

---

# Branch Strategy

The repository contains two primary branches.

| Branch | Purpose |
|---------|----------|
| main | Production code |
| staging | Staging deployment |

---

# CI/CD Workflow

The workflow is defined in:

```
.github/workflows/ci-cd.yml
```

The workflow is automatically triggered by the following events:

### Push to Staging Branch

```
git push origin staging
```

Pipeline Execution

- Checkout Repository
- Setup Python
- Install Dependencies
- Run Unit Tests
- Build Validation
- Deploy to Staging
- Health Check

---

### Production Deployment

Production deployment is triggered by publishing a GitHub Release.

Example:

```
v1.0.0
```

Pipeline Execution

- Build
- Test
- Wait for Manual Approval
- Deploy to Production
- Health Check

---

# GitHub Actions Workflow

The pipeline consists of three jobs.

## 1. Build and Test

This job performs:

- Checkout source code
- Configure Python
- Install dependencies
- Execute unit tests
- Validate application build

---

## 2. Deploy to Staging

Triggered only when changes are pushed to:

```
staging
```

Deployment Steps

- Connect to EC2 using OpenSSH
- Pull latest staging branch
- Activate Python virtual environment
- Install dependencies
- Restart Flask application
- Verify application health

---

## 3. Deploy to Production

Triggered only when:

- A GitHub Release is published

Deployment Steps

- Wait for manual approval
- Connect to EC2
- Pull latest main branch
- Restart application
- Perform health check

---

# GitHub Environments

The project uses two GitHub Environments.

## Staging

Purpose

Deploy development code for testing.

Secrets

- EC2_HOST
- EC2_USER
- EC2_SSH_KEY

---

## Production

Purpose

Deploy stable application after approval.

Features

- Required Reviewer
- Manual Deployment Approval

Secrets

- EC2_HOST
- EC2_USER
- EC2_SSH_KEY

---

# Repository Secrets

The following Repository Secrets are configured.

| Secret | Description |
|----------|-------------|
| MONGO_URI | MongoDB Atlas Connection String |
| SECRET_KEY | Flask Secret Key |

---

# Environment Secrets

Each GitHub Environment contains:

| Secret |
|----------|
| EC2_HOST |
| EC2_USER |
| EC2_SSH_KEY |

---

# Deployment Architecture

```
Developer
     │
     ▼
Git Push
     │
     ▼
GitHub Repository
     │
     ▼
GitHub Actions
     │
     ▼
Build & Test
     │
     ▼
GitHub Environment
     │
     ▼
SSH Deployment
     │
     ▼
AWS EC2
     │
     ▼
Flask Application
     │
     ▼
MongoDB Atlas
```

---

# Running Locally

Clone repository

```bash
git clone https://github.com/<username>/flask-cicd-demo.git

cd flask-cicd-demo
```

Create virtual environment

```bash
python -m venv venv
```

Activate environment

Linux

```bash
source venv/bin/activate
```

Windows

```bash
venv\Scripts\activate
```

Install dependencies

```bash
pip install -r requirements.txt
```

Run application

```bash
python app.py
```

---

# Execute Tests

Run unit tests using:

```bash
pytest -v
```

---

# Production Deployment Process

1. Merge changes into main branch.

2. Publish a GitHub Release.

3. GitHub Actions automatically starts.

4. Production deployment waits for approval.

5. Approve deployment.

6. Application is deployed to EC2.

7. Health endpoint is verified.

---

# Health Check

The deployment verifies application availability using:

```
GET /health
```

Successful response

```
HTTP 200 OK
```

---

# Security

Sensitive information is never stored inside the repository.

The project uses:

- GitHub Repository Secrets
- GitHub Environment Secrets
- OpenSSH Authentication
- Manual Production Approval

---

# Screenshots

The following screenshots are included in the project documentation.

- Repository Structure
- GitHub Environments
- Repository Secrets
- Environment Secrets
- Workflow YAML
- Successful Build
- Successful Test
- Staging Deployment
- Production Approval
- Production Deployment
- Running Flask Application

---

# Future Enhancements

Potential improvements include:

- Docker Containerization
- Kubernetes Deployment
- Infrastructure as Code (Terraform)
- Automated Security Scanning
- SonarQube Integration
- Slack/MS Teams Notifications
- Blue-Green Deployment

---

# References

- https://docs.github.com/actions
- https://docs.github.com/actions/deployment/targeting-different-environments
- https://flask.palletsprojects.com/
- https://docs.pytest.org/
- https://docs.python.org/3/
- https://docs.aws.amazon.com/ec2/
- https://www.mongodb.com/docs/atlas/

---

# Author

**Ranjeet Chirutkar**

DevOps CI/CD Project

GitHub Actions + AWS EC2 + Flask + MongoDB Atlas