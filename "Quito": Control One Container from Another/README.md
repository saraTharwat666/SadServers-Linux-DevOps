# 🐳 Scenario: Quito - Control One Container from Another

![Docker](https://media.tenor.com/soT27Z6i4gMAAAAM/hop-on-docker-desktop-docker.gif)

## 📝 Challenge Description
The objective is to manage Docker containers from within another container. Specifically, starting a stopped `nginx` container by executing commands from inside the `docker-access` container.

**Key Constraints:**
- Must not start `nginx` from the host directly.
- The `docker-access` container can be restarted or modified to gain the necessary permissions.

---

## 🔍 Investigation & Logic
By default, Docker containers are isolated and do not have access to the Docker Daemon running on the host. To grant a container management capabilities, we must bridge the gap between the container and the host's Docker engine.

The core concept used here is **Docker Socket Binding**. By mounting the host's Docker Unix socket (`/var/run/docker.sock`) into the container, the container can communicate with the Docker API of the host.

---

## 🛠️ The Fix

### 1. Enable Docker Access
To allow `docker-access` to talk to the host's Docker engine, the container must be started (or restarted) with the Docker socket mounted as a volume:
```bash
# Concept command (if recreating the container):
docker run -d --name docker-access -v /var/run/docker.sock:/var/run/docker.sock docker-access-image
```

2. Execute Command from Inside
Once the socket is mounted, I executed the start command for the nginx container from within the docker-access environment:

```Bash

docker exec docker-access docker start nginx
```


3. Verification
Verified that nginx is up and running by querying the process list from inside the controller container:
```Bash

docker exec docker-access docker ps
```


✅ Results
Socket Communication: Established via Volume Mounting.

Inter-container Control: Successfully started nginx.

Constraint Compliance: All actions performed through ```docker-access```.





















