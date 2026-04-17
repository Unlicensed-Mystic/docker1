# CI/CD Pipeline Setup with Jenkins & Docker

This document outlines the steps taken to create a fully automated CI/CD pipeline where Jenkins pulls code from GitHub, builds a new Docker image, and automatically runs the application.

---

## Architecture Overview

1. **SCM (Source Control Management):** GitHub Repository (`docker1`)
2. **CI/CD Server:** Local Jenkins container running with Docker socket access
3. **Pipeline Definition:** Declarative `Jenkinsfile` stored in the root of the repository
4. **Deployment Target:** Local Docker daemon (running the app container on port `3000`)

---

## Step 1 — Create the `Jenkinsfile`

A `Jenkinsfile` was added to the repository to define the pipeline stages.

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME    = 'akshay8969/finance-tracker'
        CONTAINER_NAME = 'finance-tracker-app'
        APP_PORT      = '3000'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling latest code from GitHub...'
                git branch: 'main', url: 'https://github.com/Unlicensed-Mystic/docker1.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest ."
            }
        }
        stage('Deploy Container') {
            steps {
                echo 'Stopping existing container...'
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm   ${CONTAINER_NAME} || true"

                echo "Starting new container on port ${APP_PORT}..."
                sh """
                    docker run -d \
                        -p ${APP_PORT}:80 \
                        --name ${CONTAINER_NAME} \
                        --restart always \
                        ${IMAGE_NAME}:latest
                """
            }
        }
        stage('Verify') {
            steps {
                sh "docker ps --filter name=${CONTAINER_NAME}"
                echo "✅ Application deployed at http://localhost:${APP_PORT}"
            }
        }
    }
}
```

---

## Step 2 — Configure Jenkins for Docker Access

Because Jenkins runs *inside* a Docker container, it needs permission to execute Docker commands on the host machine in order to build and run the application.

1. **Run Jenkins with Docker Socket mapped:**
```bash
docker run -d -p 8080:8080 -p 50000:50000 \
  --name jenkins \
  --restart=on-failure \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \ # Mapped the Docker socket
  jenkins/jenkins:lts-jdk21
```

2. **Install Docker CLI inside Jenkins container & fix permissions:**
```bash
docker exec -u root jenkins bash -c "apt-get update && apt-get install -y docker.io"
docker exec -u root jenkins chmod 666 /var/run/docker.sock
```

---

## Step 3 — Create Pipeline Job in Jenkins UI

1. Open Jenkins at `http://localhost:8080`
2. Click **New Item** → Name it `finance-tracker-pipeline` → Select **Pipeline**
3. Scroll down to the **Pipeline** section
4. Set **Definition** to **Pipeline script from SCM**
5. Set **SCM** to **Git**
6. Enter the **Repository URL**: `https://github.com/Unlicensed-Mystic/docker1.git`
7. Set **Branch Specifier** to `*/main`
8. Set **Script Path** to `Jenkinsfile`
9. Click **Save**

---

## Step 4 — Run and Verify the Pipeline

1. In Jenkins, click **Build Now** on the left menu.
2. The pipeline progresses through 4 stages:
   - **Checkout:** Pulls the newest code and `Jenkinsfile` from GitHub.
   - **Build Docker Image:** Uses `docker build` to create a fresh image.
   - **Deploy Container:** Eliminates the old instance and runs `docker run` with the newly built image.
   - **Verify:** Checks the container instance.
3. Once completed, the application goes live automatically at **[http://localhost:3000](http://localhost:3000)**.
