# Monorepo with Frontend & Backend Deployment

This repository contains a monorepo structure that includes both the frontend and backend projects. The frontend is a simple Vue.js application, while the backend is a Dockerized Node.js REST API. The project incorporates automation for CI/CD using GitHub Actions and infrastructure provisioning with Ansible to deploy the application to Cloudflare Pages and AWS EC2.

## Table of Contents
- [Monorepo with Frontend \& Backend Deployment](#monorepo-with-frontend--backend-deployment)
  - [Table of Contents](#table-of-contents)
  - [1. Project Structure](#1-project-structure)
  - [2. Infrastructure Setup with Ansible](#2-infrastructure-setup-with-ansible)
    - [Steps to Provision Your EC2 Instance:](#steps-to-provision-your-ec2-instance)
  - [3. Deployment \& CI/CD Workflows](#3-deployment--cicd-workflows)
    - [Frontend CI/CD Workflow](#frontend-cicd-workflow)
    - [Backend CI/CD Workflow](#backend-cicd-workflow)

---

## 1. Project Structure

```
├── ansible/                           # Ansible playbooks for provisioning EC2
├── frontend/                          # Vue.js frontend app
├── backend/                           # Node.js REST API backend (Dockerized)
├── .github/                           # GitHub Actions workflows for CI/CD
│   ├── workflows/                     # Folder containing frontend and backend deployment workflows
│   │   ├── frontend-deployment.yml    # Frontend deployment workflow
│   │   └── backend-deployment.yml     # Backend deployment workflow
└── README.md                          # This README file
```

## 2. Infrastructure Setup with Ansible

Before deploying the frontend or backend, you must first provision your AWS EC2 instance using Ansible. This ensures that Docker is installed on your EC2 instance to run the backend application.

### Steps to Provision Your EC2 Instance:

1. **Configure Ansible**:
   - Modify the `hosts.ini` file to include your EC2 instance details, such as the hostname, user, and private key.

2. **Run the Ansible Playbook**:
   - Navigate to the `ansible/` directory and execute the following command to install Docker on your EC2 instance:
   
   ```bash
   cd ansible
   ansible-playbook playbooks/install_docker.yml -v
   ```

This will install Docker and configure it to run the backend Docker container. Once the EC2 instance is set up with Docker, you can proceed to deploy the frontend or backend.

## 3. Deployment & CI/CD Workflows

### Frontend CI/CD Workflow
The Frontend CI/CD workflow automates the deployment process of the frontend (a Vue.js application) to Cloudflare Pages. The steps in the workflow are as follows:

1. Lint Check: Runs a lint check on the frontend code to ensure that the code follows defined standards.

2. Format Check: Verifies that the frontend code adheres to formatting rules.

3. Build: Builds the Vue.js application.

4. Deploy: Deploys the built frontend to Cloudflare Pages.

The workflow is triggered when changes are pushed to the `frontend/**` path in the `master` branch. It uses GitHub Actions to run these tasks, ensuring that the frontend is correctly linted, formatted, built, and deployed to Cloudflare Pages automatically.

### Backend CI/CD Workflow
The Backend CI/CD workflow automates the deployment of the Dockerized Node.js REST API to an AWS EC2 instance. The steps in the workflow are as follows:

1. Code Quality Checks:

   - Runs lint checks
   - Runs format checks
   - Runs tests for the backend code

2. Docker Image Build & Push:

    - Builds a production-ready Docker image of the backend.
    - Pushes the Docker image to DockerHub.

3. Deploy to AWS EC2:

    - Pulls the Docker image from DockerHub.
    - Deploys the Docker image to the EC2 instance and runs it as a container.

The backend CI/CD workflow is triggered when changes are pushed to the `backend/**` path in the `master` branch. This ensures that the backend API is linted, tested, Dockerized, and deployed automatically to the EC2 instance.

