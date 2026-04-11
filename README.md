<<<<<<< HEAD
# Personal Finance Tracker - Docker Deployment

This repository contains the source code, Docker configuration, and artifacts for the Personal Finance Tracker application as part of the Docker assignment.

## DockerHub Link

- **DockerHub Repository:** [https://hub.docker.com/r/akshay8969/finance-tracker](https://hub.docker.com/r/akshay8969/finance-tracker)

## How to Run Locally

You can easily pull and run the pre-built Docker image from DockerHub:

```bash
# Pull the latest image
docker pull akshay8969/finance-tracker:latest

# Run the container locally, exposing it on port 8080
docker run -d -p 8080:80 --name finance-tracker akshay8969/finance-tracker:latest
```

Once running, the application will be accessible at [http://localhost:8080](http://localhost:8080).

## Repository Contents

- `Dockerfile`: Multi-stage Docker build configuration for the React + Vite application.
- `nginx.conf`: Custom Nginx configuration for serving the React SPA.
- Project Source Code: Complete source code for the Personal Finance Tracker.
- `FintrackDockerScreenshots/`: Contains screenshots of the Docker image building, running locally, and pushing to DockerHub.
=======
# docker1

DockerHub - hub.docker.com/r/akshay8969/finance-tracker
>>>>>>> d5fdbb2c5aa08c80f48a9ea8ca0f514b6247fd5d
