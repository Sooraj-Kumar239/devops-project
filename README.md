

##  Project Overview

This project is doing complete automated web application deployment. The main goal of this project is to create an automated pipeline which is handling the complete setup like server configuration,
containerization, and continuous deployment.

this application hosted on AWS EC2, application automatically updated when the changes pushed on Github.

---

## Tools and Technologies Used

- **AWS EC2** – Cloud Virtual Machine hosting the application  
- **Terraform** – Infrastructure as Code (IaC) for provisioning EC2 and Security Groups  
- **Ansible** – Configuration management for installing Docker on the server  
- **Docker** – Containerization of the web application  
- **GitHub Actions** – CI/CD automation pipeline  
- **Ubuntu Server** – Operating system on EC2   

---

##  Project Architecture

The workflow of the project is structured as follows:

Code is written and tested locally.
Code is pushed to GitHub repository.
GitHub Actions workflow is triggered automatically.
The workflow connects to AWS EC2 via SSH.
Docker container is rebuilt and restarted.
Updated application becomes live on the public IP.

This automated pipeline eliminates the need for manual deployment and reduces human error.

---

##  Live Application

The application is accessible at:

```
http://13.234.202.26
```

(Note: If EC2 instance is stopped and restarted, the public IP may change unless Elastic IP is used.)

---

#  Implementation Details

---

## 1️ Infrastructure Creation (Terraform)

Terraform was used to provision the cloud infrastructure automatically instead of creating resources manually from the AWS console.

Creates AWS EC2 instance  
Creates Security Group  
Opens required ports (22 for SSH, 80 for HTTP)  

### Basic Terraform Workflow:
terraform init
terraform plan
terraform apply
Terraform ensures infrastructure consistency and allows easy recreation of resources if needed.

---

## 2️ Server Configuration (Ansible)

After the EC2 instance is created, Ansible is used to configure the server automatically.

### Reason of Ansible:

- Install Docker
- Update system packages
- Prepare environment for container deployment

Example:
ansible-playbook -i inventory install_docker.yml
This removes the need to manually SSH into the server and install dependencies.
---
## 3️ Application Containerization (Docker)
The web application is containerized using Docker to ensure portability and consistency.
### Usage of Docker?

- Ensures same environment everywhere
- Isolates application dependencies
- Simplifies deployment
- Makes scaling easier

### Docker Commands Used:

Build image:
docker build -t devops-app .

Run container:
docker run -d -p 80:80 devops-app

Docker ensures the application runs reliably inside a controlled container environment.
---

## 4️ CI/CD Automation (GitHub Actions)

The Continuous Integration and Continuous Deployment pipeline was implemented using GitHub Actions.

Whenever code is pushed to the main branch:

- Workflow triggers automatically
- Connects to EC2 using SSH
- Pulls latest code
- Stops old Docker container
- Builds new image
- Starts updated container

This makes deployment completely automated.

---

##  Security Configuration

The AWS Security Group allows:

- Port 22 (SSH)
- Port 80 (HTTP)

SSH access is secured using a private key (.pem file).

GitHub Secrets are used to store:

- EC2 public IP
- SSH private key
- Username

This ensures sensitive credentials are not exposed publicly.



