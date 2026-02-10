![Linux GIF](https://media.tenor.com/soT27Z6i4gMAAAAM/linux.gif)

# 🐧 SadServers Solutions — Mission Accomplished!

This repository documents my **hands-on solutions** for challenges from [SadServers](https://sadservers.com), focused on **real Linux system debugging**, not theoretical fixes.

The challenges range from **Medium** up to **Hard / Hack** levels and simulate real-world incidents a DevOps or Linux Admin would face in production.

---

## 🛠️ Challenges Solved

### 1️⃣ Jakarta — *It’s Always DNS* 🌐

* **Level:** Hard
* **Core Concept:** Linux Name Service Switch (NSS) & DNS Resolution Flow
* **Root Cause:** Incorrect lookup order in `/etc/nsswitch.conf`
* **Fix Applied:**

  * Adjusted hostname resolution priority to allow DNS queries beyond local files
  * Verified resolution behavior using `getent`, `dig`, and `ping`

📌 **Key Lesson:** DNS issues often come from local resolution rules — not the DNS server itself.

---

### 2️⃣ Monaco — *The Disappearing Trick* 🕵️‍♂️

* **Level:** Hack
* **Core Concept:** Linux Forensics & Process Memory Inspection
* **Root Cause:** Sensitive credentials removed from Git history but still loaded in memory
* **Fix Applied:**

  * Inspected running processes under `/proc/[PID]/environ`
  * Extracted hidden environment variables from memory
  * Used the recovered secret to restore access

📌 **Key Lesson:** Removing secrets from Git does **not** remove them from a running system.

---

## 🚀 Skills Demonstrated

* **Linux System Administration**

  * Editing and validating `/etc/` configurations
* **Networking & Troubleshooting**

  * DNS debugging, `curl` POST requests, `ip`, `ss`, and `ping`
* **Security & Forensics**

  * Process inspection via `/proc`
  * Git history analysis for leaked secrets
* **Automation & Speed Fixes**

  * Practical usage of `sed`, `grep`, and `awk`

---

## 🔗 Quick Links

* **Platform:** [https://sadservers.com](https://sadservers.com)
* **My Profile:** *(Add your SadServers profile link here)*

---

> *“Linux is not just an OS — it’s a mindset built on debugging, curiosity, and ownership.”*

