# 🚀DevOps Project:  Automated CI/CD Pipeline for a 2-Tier Flask Application on Azure

🧠Project Overview

This document outlines the step-by-step process for deploying a 2-tier web application (Flask + MySQL) on an AWS EC2 instance. The deployment is containerized using Docker and Docker Compose. A full CI/CD pipeline is established using Jenkins to automate the build and deployment process whenever new code is pushed to a GitHub repository.

⚙️Technology Stack

Frontend/Backend: Flask (Python web framework)
Database: MySQL 5.7
Containerization: Docker & Docker Compose
CI/CD: Jenkins
Cloud Platform: Microsoft Azure
Version Control: GitHub

🏢Process

1. VM Preparation
   ● Create a VM
   ● Configure Network Security Group:
     Create a security group with the following inbound rules:
       i. Type: SSH, Protocol: TCP, Port: 22, Source: Your IP
      ii. Type: HTTP, Protocol: TCP, Port: 80, Source: Anywhere (0.0.0.0/0)
     iii. Type: Custom TCP, Protocol: TCP, Port: 5000 (for Flask), Source: Anywhere (0.0.0.0/0)
      iv. Type: Custom TCP, Protocol: TCP, Port: 8080 (for Jenkins), Source: Anywhere (0.0.0.0/0)
   ● SSH into the VM
     ssh -i <private-key-file-path><username>@<PUBLIC_IP

2. Install Docker and Docker Compose on VM
   ● Update System Packages
     sudo apt update && sudo apt upgrade -y
   
   ● Install Git, Docker, and Docker Compose:
     sudo apt install git docker.io docker-compose-v2 -y
   
   ● Start and Enable Docker:
     sudo systemctl start docker
     sudo systemctl enable docker
   
   ● Add User to Docker Group (to run docker without sudo):
     sudo usermod -aG docker $USER
     newgrp docker
   
   ● Verify docker works
    docker --version
    docker compose --version

 3. Jenkins Installation
    
   ● Install Java and verify
    	sudo apt install fontconfig openjdk-21-jre
		  java --version

    ● Add Jenkins repository and install
    sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
		echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
		  /etc/apt/sources.list.d/jenkins.list > /dev/null
		sudo apt update
		sudo apt install jenkins

    ● Start and enable Jekins
    sudo systemctl start jenkins
		sudo systemctl enable jenkins
    sudo systemctl status jenkins

    Setup
    ● Retrieve the initial admin password:
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ● Access the Jenkins dashboard at http://<VM-public-ip>:8080.
    ● Paste the password, install suggested plugins, and create an admin user.
    ● Grant Jenkins Docker Permissions:
    sudo usermod -aG docker jenkins
    sudo systemctl restart jenkins

 4. Giihub Configuration
   Ensure your GitHub repository contains the following three files.

   ● Jenkinsfile
   This file contains the pipeline-as-code definition for Jenkins.
   ● Dockerfile
   This file defines the environment for the Flask application container.
   ● docker-compose.ymll
   This file defines and orchestrates the multi-container application (Flask and MySQL).
   

5. Jenkins Pipeline Creation and Execution
   ● Create a New Pipeline Job in Jenkins:
   From the Jenkins dashboard, select New Item.
   Name the project, choose Pipeline, and click OK.

  ● Configure the Pipeline:
  In the project configuration, scroll to the Pipeline section.
  Set Definition to Pipeline script from SCM.
  Choose Git as the SCM.
  Enter your GitHub repository URL.
  Verify the Script Path is Jenkinsfile.
  Save the configuration.

  ● Run the Pipeline:
  Click Build Now to trigger the pipeline manually for the first time.
  Monitor the execution through the Stage View or Console Output.

6. Verify Deployment:
   ● After a successful build, your Flask application will be accessible at http://<your-ec2-public-ip>:5000.
   ● Confirm the containers are running on the VM with docker ps.

The CI/CD pipeline is now fully operational. Any git push to the main branch of the configured GitHub repository will automatically trigger the Jenkins pipeline, which will build the new Docker image and deploy the updated application, ensuring a seamless and automated workflow from development to production.























