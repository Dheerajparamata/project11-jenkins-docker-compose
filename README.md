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
