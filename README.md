# Automated-CI-CD-Pipeline-with-Jenkins-and-Docker-k8s-AWS-parameter-store

## Overview

This project demonstrates an end-to-end **CI/CD pipeline** for a Node.js web application using modern DevOps tools and cloud concepts:

- **Web App:** Simple Node.js Express application serving a landing page.  
- **Docker:** Containerizes the web app for consistent deployment.  
- **Kubernetes:** Deploys and manages the containerized app on a Kubernetes cluster for scalability and orchestration.  
- **Jenkins:** Automates the entire build, test, and deployment process via pipeline scripts.  
- **AWS Parameter Store (Mocked):** Shows secure management of configuration and secrets via AWS Systems Manager Parameter Store.  

The pipeline automates building the Docker image, pushing it to Docker Hub, deploying to Kubernetes, and managing configuration securely — enabling faster releases and consistent environments.
