# Jenkins Local Setup Using Docker

Setting up Jenkins locally using Docker and verifying access through the browser.

---


- Docker Desktop installed and running
- Port `8080` available on localhost

---

## Step 1 — Pull & Run Jenkins Container

```bash
docker run -d \
  -p 8080:8080 \
  -p 50000:50000 \
  --name jenkins \
  --restart=on-failure \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts-jdk21
```

**Flags explained:**

| Flag | Purpose |
|------|---------|
| `-d` | Run container in detached (background) mode |
| `-p 8080:8080` | Map host port 8080 to Jenkins UI port |
| `-p 50000:50000` | Map port for Jenkins agent communication |
| `--name jenkins` | Name the container `jenkins` |
| `--restart=on-failure` | Auto-restart if the container crashes |
| `-v jenkins_home:/var/jenkins_home` | Persist Jenkins data using a Docker volume |
| `jenkins/jenkins:lts-jdk21` | Official LTS Jenkins image with JDK 21 |

---

## Step 2 — Verify Container is Running

```bash
docker ps
```

---

## Step 3 — Get the Initial Admin Password

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```
---

## Step 4 — Access Jenkins in the Browser

```
http://localhost:8080
```
---

## Step 5 — Initial Setup Wizard

1. **Install Plugins**
2. **Create Admin User** 
3. **Instance Configuration**
4. **Start using Jenkins**

---
<img width="1920" height="1019" alt="Screenshot 2026-04-16 220804" src="https://github.com/user-attachments/assets/5f3d15a4-bea6-40de-9009-269a55a9481d" />

<img width="1920" height="1013" alt="Screenshot 2026-04-16 215751" src="https://github.com/user-attachments/assets/3db82968-08ac-480f-81b4-f04c5a69455b" />

<img width="1919" height="1013" alt="image" src="https://github.com/user-attachments/assets/757fabf5-a9aa-4c91-84d8-bdcf92a6871a" />

## Useful Docker Commands for Jenkins

```bash
# Stop Jenkins
docker stop jenkins

# Start Jenkins again
docker start jenkins

# View Jenkins logs
docker logs jenkins

# Remove Jenkins container (data is safe in the volume)
docker rm jenkins

# Remove Jenkins data volume (permanent deletion)
docker volume rm jenkins_home
```

---

## Summary

| Item | Value |
|------|-------|
| **Jenkins URL** | http://localhost:8080 |
| **Docker Image** | `jenkins/jenkins:lts-jdk21` |
| **UI Port** | `8080` |
| **Agent Port** | `50000` |
| **Data Volume** | `jenkins_home` |
