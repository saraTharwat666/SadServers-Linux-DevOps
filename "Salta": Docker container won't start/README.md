# 🚀 Scenario: Salta - Node.js Dockerization

![Docker Hop](https://media.tenor.com/soT27Z6i4gMAAAAM/hop-on-docker-desktop-docker.gif)


## 📝 Challenge Description
A "dockerized" Node.js web application is located in the `/home/admin/app` directory. The goal is to create a Docker container that exposes the app on port `:8888`.

**Constraints:**
- The application must respond to `curl localhost:8888` with "Hello World!".
- Only **one** running container is allowed in the environment.

---

## 🔍 Investigation & Logic
To ensure a successful deployment, I followed these steps:
1. **Source Check:** Inspected the `/home/admin/app` directory to confirm the presence of the `Dockerfile`.
2. **Environment Cleanup:** Verified that no other processes or containers were occupying port `8888` on the host machine.
3. **Port Mapping:** Determined that the host port `8888` must be mapped to the container's internal port to allow external access via `curl`.

---

## 🛠️ The Fix

### 1. Build the Image
Navigate to the source directory and build the image:
```bash
cd /home/admin/app
docker build -t node-app-salta .
```

2. Run the Container

```bash
docker run -d -p 8888:8888 --name salta-container node-app-salta
```


2. Run the Container
Start the container in detached mode with the required port mapping:

```Bash

docker run -d -p 8888:8888 --name salta-container node-app-salta
```

✅ Verification
Test the running application:

```Bash

curl localhost:8888
# Response: Hello World!




























