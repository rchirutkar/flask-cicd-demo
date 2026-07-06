# Flask CI/CD Pipeline using Jenkins, GitHub, AWS EC2 & MongoDB Atlas

![Python](https://img.shields.io/badge/Python-3.12-blue)
![Flask](https://img.shields.io/badge/Flask-Web%20Application-black)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red)
![AWS](https://img.shields.io/badge/AWS-EC2-orange)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-green)
![License](https://img.shields.io/badge/License-MIT-blue)

---

# Project Overview

This project demonstrates the implementation of a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Flask web application.

The application is integrated with MongoDB Atlas and automatically built, tested, and deployed using Jenkins whenever changes are pushed to the GitHub repository.

The deployment target is an AWS EC2 Ubuntu instance where the application runs as a Linux service.

---

# Objectives

- Develop a Flask web application.
- Store application data in MongoDB Atlas.
- Implement automated testing using Pytest.
- Configure a Jenkins CI/CD pipeline.
- Deploy the application automatically to AWS EC2.
- Implement automatic build triggering.
- Configure email notifications for build success and failure.

---

# Technology Stack

| Technology        | Purpose                   |
|-------------------|---------------------------|
| Python 3          | Programming Language      |
| Flask             | Web Framework             |
| MongoDB Atlas     | Cloud Database            |
| Flask-PyMongo     | MongoDB Integration       |
| Pytest            | Unit Testing              |
| Git               | Version Control           |
| GitHub            | Source Code Repository    |
| Jenkins           | Continuous Integration    |
| AWS EC2 Ubuntu    | Deployment Server         |
| Gmail SMTP        | Email Notifications       |

---

# Project Architecture

            Developer

                ↓

            GitHub Repository

                ↓

            Jenkins Pipeline

                ↓

            Checkout

                ↓

            Install Dependencies

                ↓

            Run Pytest

                ↓

            Deploy to EC2

                ↓

            Flask Application

                ↓

            MongoDB Atlas

                ↓

            Email Notification

---

# Overview

                    Developer
                        │
                        │
                  Git Push (main)
                        │
                        ▼
               GitHub Repository
                        │
             SCM Polling (Jenkins)
                        │
                        ▼
               Jenkins Pipeline Server
                        │
     ┌──────────────────┼──────────────────┐
     │                  │                  │
     ▼                  ▼                  ▼
 Checkout            Build              Test
 (Git Clone)   (Install Packages)    (Pytest)
                        │
                        ▼
                   Deploy Stage
                        │
                 SSH to AWS EC2
                        │
                        ▼
              AWS EC2 Ubuntu Server
                        │
            Git Pull (origin/main)
                        │
            Activate Python venv
                        │
          Install Dependencies
                        │
          Restart Flask Application
                        │
             Health Check (/health)
                        │
                        ▼
             Flask Web Application
                        │
                        ▼
              MongoDB Atlas Cluster

───────────────────────────────────────────────

        Gmail SMTP
             ▲
             │
     Success / Failure
      Email Notification
             │
             ▼
          Developer

---

# Repository Structure

```text
flask-cicd-demo/
│
├── tests/
│   └── test_app.py
│
├── templates/
│
├── app.py
├── Jenkinsfile
├── requirements.txt
├── README.md
├── ReadmeCICD.md
└── .gitignore
```

---

# Prerequisites

- Python 3.12+
- Git
- Jenkins
- AWS EC2 Ubuntu
- MongoDB Atlas Account
- Gmail Account
- SSH Key Pair

---

# Installation

Clone the repository

```bash
git clone https://github.com/<username>/flask-cicd-demo.git

cd flask-cicd-demo
```

Create Virtual Environment

```bash
python3 -m venv venv
```

Activate

Linux

```bash
source venv/bin/activate
```

Windows

```cmd
venv\Scripts\activate
```

Install Requirements

```bash
pip install -r requirements.txt
```

---

# Environment Variables

Create a `.env` file.

```
MONGO_URI=your_mongodb_connection_string

SECRET_KEY=your_secret_key
```

---

# Running the Application

```bash
python app.py
```

Open

```
http://localhost:8000
```

Health Endpoint

```
http://localhost:8000/health
```

---

# Running Tests

```bash
python -m pytest -v
```

---

# Jenkins Pipeline

The Jenkins pipeline consists of four stages.

- Checkout
- Build
- Test
- Deploy

The deployment stage performs the following tasks:

- Pull latest source code
- Activate Python virtual environment
- Install dependencies
- Restart Flask application
- Verify health endpoint

---

# AWS Deployment

The application is deployed on an Ubuntu EC2 instance.

Deployment steps include:

- Git Pull
- Install Dependencies
- Restart Flask Service
- Health Check

The application is accessible using the EC2 Public IP.

---

# Build Trigger

Automatic build execution is configured using Jenkins SCM Polling.

```
H/2 * * * *
```

Whenever changes are pushed to the main branch, Jenkins detects repository updates and automatically starts the pipeline.

---

# Email Notifications

The pipeline sends email notifications after every build.

Notifications include:

- Build Success
- Build Failure

Each email contains:

- Job Name
- Build Number
- Build Status
- Build URL

---

# Features

- Flask Web Application
- MongoDB Atlas Integration
- Automated Unit Testing
- Jenkins CI/CD Pipeline
- AWS EC2 Deployment
- Health Endpoint
- SCM Polling
- Email Notification
- Secure Credentials Management

---

# Future Enhancements

- Docker Containerization
- Kubernetes Deployment
- GitHub Actions Pipeline
- Nginx Reverse Proxy
- HTTPS using Let's Encrypt
- SonarQube Integration
- Code Coverage Reports

---

# Author

**Ranjeet Chirutkar**

DevOps | Cloud | CI/CD | Python | AWS
