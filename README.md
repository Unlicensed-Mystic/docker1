# Personal Finance Tracker - Docker Deployment

This repository contains the source code, Docker configuration, and deployment artifacts for the Personal Finance Tracker application as part of the Docker + AWS EC2 assignment.

---

## 🌐 Live Deployment (AWS EC2)

**Public URL:** [http://16.171.241.129](http://16.171.241.129)

> Hosted on an AWS EC2 `t2.micro` instance (Ubuntu 24.04 LTS) with Docker.

---

## 🐳 DockerHub

- **DockerHub Repository:** [https://hub.docker.com/r/akshay8969/finance-tracker](https://hub.docker.com/r/akshay8969/finance-tracker)

```bash
docker pull akshay8969/finance-tracker:latest
```

---

## ☁️ AWS EC2 Deployment — Commands Used

### 1. SSH into EC2 Instance
```bash
ssh -i "finance-key.pem" ubuntu@16.171.241.129
```

### 2. Install Docker on EC2
```bash
sudo apt-get update -y
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
newgrp docker
```

### 3. Pull Image from DockerHub & Run
```bash
docker pull akshay8969/finance-tracker:latest
docker run -d -p 80:80 --name finance-tracker --restart always akshay8969/finance-tracker:latest
```

### 4. Verify Container is Running
```bash
docker ps
```

App is now accessible at: **http://16.171.241.129**

---

## 💻 Run Locally (from DockerHub)

```bash
docker pull akshay8969/finance-tracker:latest
docker run -d -p 8080:80 --name finance-tracker akshay8969/finance-tracker:latest
```

Visit: [http://localhost:8080](http://localhost:8080)

---

## 📁 Repository Contents

| File/Folder | Description |
|---|---|
| `Dockerfile` | Multi-stage build (Node 20 Alpine → Nginx Alpine) |
| `nginx.conf` | Custom Nginx config for React SPA routing |
| `src/` | React + Vite application source code |
| `FintrackDockerScreenshots/` | Screenshots of build, run, and push steps |
