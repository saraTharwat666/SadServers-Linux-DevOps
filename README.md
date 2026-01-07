# 🛠️ System Debugging & Infrastructure Lab

![Docker Magic](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNHRydm9ueGZ6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3JmJmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZ25vcmUmY3Q9Zw/3ohzdTbc97tLR87O3S/giphy.gif)

## 📌 What's happening here?
This repository is a collection of hands-on solutions for broken infrastructure. Each directory contains a deep dive into a specific system failure—ranging from Docker container crashes and networking bottlenecks to PostgreSQL replication mismatches.

The focus is on **Root Cause Analysis (RCA)**: finding why the system is "sad" and implementing the most efficient fix to bring it back to a healthy state.

---

## ⚙️ The Workflow
![DevOps Life]([https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExM3Z6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3R6Z3JmJmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZ25vcmUmY3Q9Zw/VGG8UY1nEl66Y/giphy.gif](https://i.pinimg.com/originals/f5/5e/80/f55e8059ea945abfd6804b887dd4a0af.gif))

- **Log Hunting:** Digging through `journalctl` and `docker logs` to spot the error.
- **Environment Auditing:** Checking system parameters, file descriptors, and network ports.
- **Patching & Optimization:** Fixing the config and ensuring it doesn't break again.

---
**Maintained by a DevOps Engineer focused on keeping the lights on.**
