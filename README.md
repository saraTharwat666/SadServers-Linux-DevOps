# 🛠️ DevOps & Infrastructure Troubleshooting Lab

![Docker Deployment](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNHRydm9ueGZ6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3JmJmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZ25vcmUmY3Q9Zw/3ohzdTbc97tLR87O3S/giphy.gif)

## 📌 Overview
Documentation of real-world infrastructure fixes and system debugging. This repository focuses on Root Cause Analysis (RCA) for Dockerized environments, Linux systems, and Database clusters.

---

## 📂 Challenge Registry

| Scenario | Level | Stack | Documentation |
| :--- | :--- | :--- | :--- |
| **Helsingør** | Medium | PostgreSQL, Docker-Compose | [Analysis & Fix](./Helsingor-Postgres-Replication/) |
| **Salta** | Medium | Docker, Node.js | [Analysis & Fix](./Salta-Nodejs-App/) |
| **Saint John** | Easy | Linux, Process Management | [Analysis & Fix](./Saint-John-Log-Killer/) |

---

## ⚙️ Core Methodology
![DevOps Life](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNHRydm9ueGZ6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3JmJmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZ25vcmUmY3Q9Zw/l0HlOnf0S70qJshQ4/giphy.gif)

- **Analyze:** Investigating logs (`journalctl`, `docker logs`) and system states.
- **Isolate:** Identifying the failure point (Networking, Permissions, or Config mismatch).
- **Resolve:** Implementing optimized fixes and verifying system stability.

---
**Focusing on high-availability and system reliability.**
