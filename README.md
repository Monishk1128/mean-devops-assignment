# MEAN Stack Application – Containerization & CI/CD Deployment

## Project Overview

This project demonstrates the complete containerization, deployment, and CI/CD automation of a full-stack MEAN (MongoDB, Express, Angular, Node.js) application.

The objective of this assignment was to:

- Containerize both frontend and backend using Docker  
- Push Docker images to Docker Hub  
- Deploy the application on an Ubuntu EC2 instance  
- Configure Nginx as a reverse proxy (Port 80)  
- Implement CI/CD using GitHub Actions  
- Automate image updates and deployment on every push  

The final application is deployed on AWS and accessible publicly via the EC2 public IP address.

---

## Tech Stack Used

- MongoDB  
- Express.js  
- Angular  
- Node.js  
- Docker & Docker Compose  
- GitHub Actions  
- AWS EC2 (Ubuntu 22.04)  
- Nginx  

---

# Step-by-Step Setup and Deployment Instructions

## 1. Local Project Setup

After extracting the provided project files, the structure included:

- `frontend/`
- `backend/`

Install dependencies:

### Backend
```bash
cd backend
npm install
```

### Frontend
```bash
cd frontend
npm install
```

---

## 2. Dockerization

### Backend Dockerfile

- Uses Node.js base image
- Installs dependencies
- Exposes port 5000
- Starts backend server

Location:
```
backend/Dockerfile
```

### Frontend Dockerfile (Multi-Stage Build)

- First stage: Build Angular app
- Second stage: Serve using Nginx
- Exposes port 80

Location:
```
frontend/Dockerfile
```

---

## 3. Docker Compose Configuration

File:
```
docker-compose.yml
```

Services:
- MongoDB
- Backend
- Frontend

Run locally:

```bash
docker-compose up --build
```

---

## 4. Docker Hub Integration

Build images:

```bash
docker build -t <dockerhub-username>/backend:latest ./backend
docker build -t <dockerhub-username>/frontend:latest ./frontend
```

Push images:

```bash
docker push <dockerhub-username>/backend:latest
docker push <dockerhub-username>/frontend:latest
```

---

## 5. AWS EC2 Deployment

Create Ubuntu 22.04 EC2 instance.

Allow:
- Port 22 (SSH)
- Port 80 (HTTP)

Install Docker:

```bash
sudo apt update
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo systemctl enable docker
```

Run containers:

```bash
docker-compose up -d
```

---

## 6. Nginx Reverse Proxy Setup

Install Nginx:

```bash
sudo apt install nginx -y
```

Configure reverse proxy in:

```
/etc/nginx/sites-available/default
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

Access application via:

```
http://<EC2_PUBLIC_IP>
```

---

## 7. CI/CD with GitHub Actions

Workflow location:

```
.github/workflows/deploy.yml
```

Pipeline runs on push to `master` branch.

Steps:
1. Checkout code
2. Login to Docker Hub
3. Build backend image
4. Build frontend image
5. Push images
6. SSH into EC2
7. Pull latest images
8. Restart containers

---

## GitHub Secrets Used

- DOCKER_USERNAME  
- DOCKER_PASSWORD (Access Token)  
- EC2_HOST  
- EC2_USER  
- EC2_SSH_KEY  

---

## Automated Deployment Flow

On every push:

- Images build automatically
- Images push to Docker Hub
- EC2 pulls latest images
- Containers restart automatically

---

## Run Locally

```bash
git clone <repository-url>
cd mean-devops-assignment
docker-compose up --build
```

Access locally:

```
http://localhost:4200
```

---

## Conclusion

This project demonstrates complete containerization, cloud deployment, reverse proxy configuration, and CI/CD automation following modern DevOps best practices.