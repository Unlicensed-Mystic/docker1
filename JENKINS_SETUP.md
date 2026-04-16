# Jenkins Local Setup Using Docker

Setting up Jenkins locally using Docker and verifying access through the browser.

---

## Prerequisites

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

Expected output:
```
CONTAINER ID   IMAGE                      COMMAND                  CREATED         STATUS         PORTS                                              NAMES
1ead199d7175   jenkins/jenkins:lts-jdk21  ...                      1 minute ago    Up 1 minute    0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
```

---

## Step 3 — Get the Initial Admin Password

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

This will output a 32-character alphanumeric password, for example:
```
942462201b04486eafc3d7ad4e5df2af
```

---

## Step 4 — Access Jenkins in the Browser

Open your browser and navigate to:

```
http://localhost:8080
```

You will see the **"Unlock Jenkins"** page. Paste the password retrieved in Step 3 and click **Continue**.

---

## Step 5 — Initial Setup Wizard

1. **Install Plugins** — Select **"Install suggested plugins"** and wait for installation to complete.
2. **Create Admin User** — Fill in your desired username, password, full name, and email.
3. **Instance Configuration** — Keep the default URL (`http://localhost:8080`) and click **Save and Finish**.
4. Click **Start using Jenkins** — Jenkins dashboard will load. ✅

---

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
