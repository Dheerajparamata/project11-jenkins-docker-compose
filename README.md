# 🚀 Project 11 – Jenkins CI/CD with Docker Compose

## 📌 Project Overview

This project demonstrates a complete CI/CD pipeline using:

**GitHub → Jenkins → Docker → Docker Compose → Nginx**

The project automates the deployment of a simple Nginx web application on an Ubuntu cloud VM.

Jenkins retrieves the source code from GitHub, validates the Docker Compose configuration, deploys the Nginx container, and automatically verifies that the application is running successfully.

During the project, a real-world Docker networking issue was encountered where port `8080` was already being used by Jenkins. The issue was identified and resolved by changing the Nginx host port from `8080` to `8081`.

---

## 🏗️ Project Architecture

```text
                         Developer
                             |
                             | git push
                             v
                    +----------------+
                    |     GitHub     |
                    |   Repository   |
                    +-------+--------+
                            |
                            | Checkout
                            v
                    +----------------+
                    |    Jenkins     |
                    |     CI/CD      |
                    +-------+--------+
                            |
                            | Pipeline
                            v
                +-------------------------+
                |   Validate Docker       |
                |       Compose           |
                +-----------+-------------+
                            |
                            v
                +-------------------------+
                |    Docker Compose       |
                |      Deployment         |
                +-----------+-------------+
                            |
                            v
                +-------------------------+
                |    Nginx Container      |
                |    project11-nginx      |
                +-----------+-------------+
                            |
                            | Port 8081
                            v
                    +----------------+
                    |  Web Browser   |
                    | Nginx Website  |
                    +----------------+
---

# 🔄 CI/CD Workflow

The complete CI/CD workflow is:

GitHub
   ↓
Jenkins
   ↓
Checkout Source Code
   ↓
Validate Docker Compose
   ↓
docker compose down
   ↓
docker compose up -d
   ↓
Check Container Status
   ↓
Test Application using curl
   ↓
Deployment Successful

---

# 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Git | Version Control |
| GitHub | Source Code Repository |
| Jenkins | CI/CD Automation |
| Docker | Containerization |
| Docker Compose | Container Deployment |
| Nginx | Web Server |
| Ubuntu 24.04 | Operating System |
| HTML | Web Application |
| Google Cloud VM | Cloud Infrastructure |

---

# ☁️ Environment Details

| Component | Configuration |
|---|---|
| Operating System | Ubuntu 24.04 |
| Git | 2.43.0 |
| Docker | 29.1.3 |
| Docker Compose | 2.40.3 |
| Jenkins | Jenkins CI/CD Server |
| Nginx | nginx:latest |
| Jenkins Port | 8080 |
| Nginx Port | 8081 |

---

# 📁 Project Structure

```text
project11-jenkins-docker-compose/
│
├── Jenkinsfile
├── docker-compose.yml
├── index.html
├── README.md
│
└── screenshots/
    ├── jenkins-dashboard.png
    ├── jenkins-job.png
    ├── jenkins-pipeline.png
    ├── docker-compose.png
    ├── jenkins-success.png
    └── nginx-webpage.png
