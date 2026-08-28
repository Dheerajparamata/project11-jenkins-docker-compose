# Project 11 – Jenkins CI/CD with Docker Compose

## 📌 Project Overview

This project demonstrates a complete **CI/CD pipeline using Jenkins, GitHub, Docker, and Docker Compose**.

The objective of this project is to automate the deployment of an Nginx web application. The source code is maintained in GitHub, Jenkins automatically retrieves the code, validates the Docker Compose configuration, deploys the application using Docker Compose, and finally verifies that the Nginx web server is running successfully.

This project also demonstrates troubleshooting a real-world Docker networking issue where port `8080` was already occupied by Jenkins.

---

## 🎯 Objectives

The main objectives of this project are:

- Configure Jenkins as a CI/CD server
- Integrate Jenkins with GitHub
- Use Git for source code management
- Install and configure Docker
- Install and use Docker Compose
- Allow Jenkins to execute Docker commands
- Create a Docker Compose configuration
- Deploy an Nginx web server using Docker Compose
- Automate deployment using a Jenkins Pipeline
- Validate Docker Compose configuration
- Verify the deployed application automatically
- Troubleshoot and resolve a Docker port conflict
- Access the deployed application through a web browser

---

# 🏗️ Project Architecture

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
                |     Jenkins    |
                |    CI/CD       |
                +-------+--------+
                        |
                        v
              +---------------------+
              | Validate Docker     |
              | Compose             |
              +----------+----------+
                         |
                         v
              +---------------------+
              | Docker Compose      |
              | Deployment          |
              +----------+----------+
                         |
                         v
              +---------------------+
              |   Nginx Container   |
              |  project11-nginx    |
              +----------+----------+
                         |
                         | Port 8081
                         v
                +----------------+
                |   Web Browser  |
                | Nginx Website  |
                +----------------+
🔄 CI/CD Workflow
The complete CI/CD workflow is:
GitHub
   |
   v
Jenkins
   |
   v
Checkout Source Code
   |
   v
Validate Docker Compose
   |
   v
docker compose down
   |
   v
docker compose up -d
   |
   v
Check Container Status
   |
   v
Test Application using curl
   |
   v
Deployment Successful
🛠️ Technologies Used
Technology	Purpose
GitHub	Source code repository
Git	Version control
Jenkins	CI/CD automation
Docker	Containerization
Docker Compose	Application deployment
Nginx	Web server
Ubuntu	Operating system
Bash	Command-line automation
HTML	Web application


☁️ Environment
The project was deployed on an Ubuntu cloud virtual machine.
Environment Details
Operating System : Ubuntu 24.04
Git              : 2.43.0
Docker           : 29.1.3
Docker Compose   : 2.40.3
Jenkins          : Jenkins CI/CD Server
Web Server       : Nginx
Jenkins Port     : 8080
Nginx Port       : 8081
📂 Project Structure
project11-jenkins-docker-compose/
│
├── Jenkinsfile
├── docker-compose.yml
├── index.html
├── README.md
└── screenshots/
    ├── jenkins-dashboard.png
    ├── jenkins-job.png
    ├── jenkins-pipeline.png
    ├── docker-compose.png
    ├── jenkins-success.png
    └── nginx-webpage.png
1️⃣ Create the Project Directory
The project directory was created on the Ubuntu VM.
mkdir project11-jenkins-docker-compose
Move into the project directory:
cd project11-jenkins-docker-compose
2️⃣ Check the Environment
The operating system was verified using:
lsb_release -a
Git was checked using:
git --version
Docker was checked using:
docker --version
Docker Compose was checked using:
docker compose version
The installed Docker version was:
Docker version 29.1.3
The installed Docker Compose version was:
Docker Compose version 2.40.3
3️⃣ Install and Start Docker
Docker was enabled and started using:
sudo systemctl enable --now docker
Docker status can be checked using:
sudo systemctl status docker
Docker version:
docker --version
4️⃣ Configure Jenkins for Docker
Jenkins needs permission to communicate with Docker.
The Jenkins user was added to the Docker group:
sudo usermod -aG docker jenkins
Jenkins was restarted:
sudo systemctl restart jenkins
Jenkins status was checked:
sudo systemctl status jenkins --no-pager
The Jenkins service was successfully running:
Active: active (running)
5️⃣ Verify Docker Access from Jenkins
Docker access for the Jenkins user was checked using:
sudo -u jenkins docker --version
Docker Compose access was checked using:
sudo -u jenkins docker compose version
Docker containers were checked using:
sudo -u jenkins docker ps
This confirmed that Jenkins could communicate with Docker.
6️⃣ Create the HTML Application
The application is a simple HTML page served through Nginx.
The file used is:
index.html
Contents:
<!DOCTYPE html>
<html>
<head>
    <title>Project 11 - Jenkins Docker Compose</title>
</head>
<body>
    <h1>Project 11 - Jenkins CI/CD</h1>
    <h2>Docker Compose Deployment</h2>
    <p>Nginx successfully deployed using Jenkins and Docker Compose.</p>
</body>
</html>
7️⃣ Create Docker Compose Configuration
The Docker Compose configuration is stored in:
docker-compose.yml
The configuration used is:
services:
  nginx:
    image: nginx:latest
    container_name: project11-nginx

    ports:
      - "8081:80"

    volumes:
      - ./index.html:/usr/share/nginx/html/index.html:ro

    restart: unless-stopped
🐳 Docker Compose Configuration Explanation
Nginx Image
image: nginx:latest
The official Nginx Docker image is used.
Container Name
container_name: project11-nginx
The container is named:
project11-nginx
Port Mapping
ports:
  - "8081:80"
This maps:
Host Port 8081
      |
      v
Container Port 80
Nginx listens on port 80 inside the container.
The host exposes it through port 8081.
Volume
volumes:
  - ./index.html:/usr/share/nginx/html/index.html:ro
The local HTML file is mounted into the Nginx web directory.
The ro option means the file is mounted as read-only.
Restart Policy
restart: unless-stopped
Docker automatically restarts the container unless it has been manually stopped.
8️⃣ Create the Jenkins Pipeline
The Jenkins pipeline is defined in:
Jenkinsfile
The complete Jenkinsfile is:
pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Docker Compose') {
            steps {
                sh 'docker compose config'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose down'
                sh 'docker compose up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker compose ps'
                sh 'curl -f http://localhost:8081'
            }
        }
    }

    post {

        success {
            echo 'Project 11 deployment completed successfully!'
        }

        failure {
            echo 'Project 11 deployment failed.'
        }
    }
}
🔍 Jenkins Pipeline Stages
Stage 1 – Checkout
Jenkins retrieves the source code from GitHub.
stage('Checkout') {
    steps {
        checkout scm
    }
}
The repository used for this project is:
project11-jenkins-docker-compose
Stage 2 – Validate Docker Compose
Jenkins executes:
docker compose config
This verifies that the Docker Compose configuration is valid.
The pipeline successfully displayed:
name: project11-jenkins-docker-compose
services:
  nginx:
    image: nginx:latest
The configured port was:
8081 -> 80
Stage 3 – Deploy with Docker Compose
First, Jenkins removes the previous deployment:
docker compose down
Then Jenkins starts the new deployment:
docker compose up -d
This creates the Docker network and Nginx container.
The container created is:
project11-nginx
Stage 4 – Verify Deployment
Jenkins checks the running container:
docker compose ps
Expected result:
NAME              IMAGE          SERVICE   STATUS
project11-nginx   nginx:latest   nginx     Up
The port mapping is:
0.0.0.0:8081 -> 80
Jenkins then tests the web application:
curl -f http://localhost:8081
The HTML page is returned successfully.
9️⃣ Configure Git
Git was initialized inside the project directory:
git init
The main branch was configured:
git branch -M main
The GitHub repository was configured as the remote repository:
git remote add origin git@github.com:Dheerajparamata/project11-jenkins-docker-compose.git
The remote repository was verified:
git remote -v
🔐 GitHub Authentication
A GitHub Personal Access Token was configured for Git operations.
The token used for authentication was a GitHub classic personal access token.
The token was used for authentication only and was not stored inside the project source code.
📤 Push Project to GitHub
The project files were added:
git add .
The project was committed:
git commit -m "Complete Project 11 Jenkins Docker Compose CI/CD"
The project was pushed to GitHub:
git push -u origin main
The GitHub repository is:
Dheerajparamata/project11-jenkins-docker-compose
🔗 Jenkins and GitHub Integration
Jenkins was configured to use the GitHub repository as the source control repository.
Jenkins retrieves:
Jenkinsfile
docker-compose.yml
index.html
from GitHub.
The pipeline then executes the instructions defined inside the Jenkinsfile.
⚠️ Problem Encountered: Port 8080 Conflict
During the first deployment, the Jenkins pipeline failed.
The error was:
failed to bind host port 0.0.0.0:8080/tcp:
address already in use
The reason was that Jenkins itself was already using port:
8080
The Nginx Docker container was also configured to use:
8080:80
Therefore, Docker could not bind Nginx to port 8080.
🔧 Troubleshooting the Port Conflict
The port being used by Jenkins was checked using:
sudo ss -ltnp | grep :8080
The result showed that Jenkins was listening on port 8080.
Therefore, two services were attempting to use the same host port:
Jenkins → 8080
Nginx   → 8080
This caused the deployment failure.
✅ Solution
The Nginx host port was changed from:
8080:80
to:
8081:80
The Docker Compose file was updated:
ports:
  - "8081:80"
The Jenkins verification command was also changed to:
curl -f http://localhost:8081
The new configuration became:
Jenkins → Port 8080
Nginx   → Port 8081
There was no longer a port conflict.
🚀 Final Deployment
After fixing the port conflict, the Jenkins pipeline was executed again.
Docker Compose successfully created the network:
project11-jenkins-docker-compose_default
The Nginx container was created:
project11-nginx
The container started successfully:
Container project11-nginx Started
📊 Container Verification
Jenkins executed:
docker compose ps
The final result showed:
NAME              IMAGE          COMMAND                  SERVICE
project11-nginx   nginx:latest   "/docker-entrypoint..." nginx
Status:
Up
Port:
0.0.0.0:8081->80/tcp
This confirmed that the Nginx container was running successfully.
🧪 Application Verification
Jenkins automatically executed:
curl -f http://localhost:8081
The request returned the application HTML:
<!DOCTYPE html>
<html>
<head>
    <title>Project 11 - Jenkins Docker Compose</title>
</head>
<body>
    <h1>Project 11 - Jenkins CI/CD</h1>
    <h2>Docker Compose Deployment</h2>
    <p>Nginx successfully deployed using Jenkins and Docker Compose.</p>
</body>
</html>
The successful curl response confirmed that the Nginx web server was working correctly.
🎉 Jenkins Final Result
The final Jenkins build completed successfully.
[Pipeline] End of Pipeline
Finished: SUCCESS
Jenkins displayed:
Project 11 deployment completed successfully!
🌐 Access the Application
The application can be accessed from a web browser using:
http://<VM_EXTERNAL_IP>:8081
For example:
http://YOUR_VM_EXTERNAL_IP:8081
The browser displays:
Project 11 - Jenkins CI/CD

Docker Compose Deployment

Nginx successfully deployed using Jenkins and Docker Compose.
🔥 Firewall Configuration
If the application is running on a cloud VM, TCP port 8081 must be allowed through the VM's firewall/security rules.
The required rule is:
Protocol : TCP
Port     : 8081
After allowing the port, the application can be accessed using:
http://<VM_EXTERNAL_IP>:8081
📸 Screenshots
Jenkins Dashboard
The Jenkins server was successfully installed and running.

Jenkins Job
The Project 11 Jenkins job was created and configured.

Jenkins Pipeline Configuration
The Jenkins pipeline was configured to use the GitHub repository.

Docker Compose Configuration
Docker Compose was configured to deploy the Nginx container.

Jenkins Successful Build
After resolving the port conflict, the Jenkins pipeline completed successfully.

Nginx Web Application
The deployed Nginx application was successfully accessed through the browser.

📋 Pipeline Execution Summary
Stage	Result	Description
Checkout	✅ SUCCESS	Retrieved project from GitHub
Validate Docker Compose	✅ SUCCESS	Validated Compose configuration
Deploy with Docker Compose	✅ SUCCESS	Started Nginx container
Verify Deployment	✅ SUCCESS	Verified container and application
Final Pipeline	✅ SUCCESS	Deployment completed


💻 Important Commands
Linux
ls
pwd
cd ~/project11-jenkins-docker-compose
Git
git init
git branch -M main
git status
git add .
git commit -m "Complete Project 11 Jenkins Docker Compose CI/CD"
git remote -v
git push -u origin main
Docker
docker --version
docker ps
docker compose version
docker compose config
docker compose down
docker compose up -d
docker compose ps
Jenkins
sudo systemctl enable --now jenkins
sudo systemctl status jenkins --no-pager
sudo systemctl restart jenkins
sudo usermod -aG docker jenkins
Docker Permission Testing
sudo -u jenkins docker --version
sudo -u jenkins docker compose version
sudo -u jenkins docker ps
Application Testing
curl -f http://localhost:8081
Port Checking
sudo ss -ltnp | grep :8080
📚 What I Learned
Through this project, I gained practical experience with:
Jenkins
- Installing Jenkins
- Starting and managing Jenkins
- Creating Jenkins Pipeline jobs
- Using Jenkinsfiles
- Running shell commands from Jenkins
- Integrating Jenkins with GitHub
Docker
- Installing Docker
- Managing Docker containers
- Giving Jenkins access to Docker
- Running Nginx using Docker
Docker Compose
- Creating a docker-compose.yml
- Defining services
- Mapping ports
- Mounting volumes
- Starting and stopping services
- Validating Compose configuration
Git and GitHub
- Initializing Git repositories
- Creating commits
- Configuring GitHub remotes
- Pushing source code to GitHub
- Using GitHub as a CI/CD source repository
Troubleshooting
I also learned how to identify and resolve a Docker port conflict.
The issue was:
Jenkins → 8080
Nginx   → 8080
The solution was:
Jenkins → 8080
Nginx   → 8081
🧠 DevOps Concepts Demonstrated
This project demonstrates several important DevOps concepts:
Continuous Integration
GitHub stores the application source code and Jenkins retrieves the latest version.
Continuous Deployment
Jenkins automatically deploys the application using Docker Compose.
Infrastructure as Code
Docker Compose defines the application environment and deployment configuration.
Automation
The Jenkins pipeline automates:
Checkout
Validation
Deployment
Verification
Automated Testing
The application is automatically tested using:
curl -f http://localhost:8081
🔐 Security Considerations
Sensitive credentials such as GitHub Personal Access Tokens should never be committed to GitHub.
Do not place tokens directly inside:
Jenkinsfile
docker-compose.yml
README.md
Jenkins credentials should be used whenever authentication is required.
📈 Future Improvements
This project can be extended by adding:
- GitHub Webhooks
- Automatic Jenkins builds after GitHub pushes
- Docker image versioning
- Docker Hub integration
- HTTPS using SSL/TLS
- Nginx reverse proxy
- Environment variables
- Jenkins credentials management
- Automated rollback
- Docker image scanning
- Automated security scanning
- Monitoring using Prometheus and Grafana
- Deployment to Kubernetes
- Blue-Green deployment
- Production-grade CI/CD pipeline
🏆 Project Outcome
The final CI/CD pipeline successfully automates the deployment of an Nginx web application.
The final architecture is:
                    GitHub
                       |
                       v
                   Jenkins
                       |
                       v
              Docker Compose
                       |
                       v
                Nginx Container
                       |
                       v
                  Port 8081
                       |
                       v
                  Web Browser
The final Jenkins pipeline result was:
SUCCESS
The application was successfully deployed and verified.
✅ Project Checklist
☑ Ubuntu VM configured
☑ Git installed
☑ Docker installed
☑ Docker Compose installed
☑ Jenkins installed
☑ Jenkins service running
☑ Jenkins added to Docker group
☑ Docker access verified from Jenkins
☑ Git repository created
☑ GitHub repository configured
☑ index.html created
☑ docker-compose.yml created
☑ Jenkinsfile created
☑ Jenkins Pipeline configured
☑ Docker Compose validated
☑ Nginx container deployed
☑ Port conflict identified
☑ Port conflict resolved
☑ Nginx moved to port 8081
☑ Container verified
☑ Application verified using curl
☑ Browser access verified
☑ Jenkins build completed successfully
🎯 Final Result
Project 11 – Jenkins CI/CD with Docker Compose was successfully completed.
The project demonstrates a practical DevOps workflow using:
Git
+
GitHub
+
Jenkins
+
Docker
+
Docker Compose
+
Nginx
The application was automatically deployed and verified through Jenkins.
🚀 GitHub → Jenkins → Docker Compose → Nginx → Web Browser
👨‍💻 Author
Dheeraj Paramata
GitHub:
Dheerajparamata
Repository:
project11-jenkins-docker-compose
⭐ Project Status
Completed Successfully ✅
Jenkins CI/CD              ✅
GitHub Integration         ✅
Docker                     ✅
Docker Compose             ✅
Nginx Deployment           ✅
Automated Verification     ✅
Port Conflict Resolution   ✅
Final Jenkins Build        ✅ SUCCESS
⭐ Thank You
Thank you for checking out this project!
This project was created as part of my DevOps learning journey to gain hands-on experience with CI/CD, Jenkins, Docker, Docker Compose, GitHub, and cloud-based Linux environments.
