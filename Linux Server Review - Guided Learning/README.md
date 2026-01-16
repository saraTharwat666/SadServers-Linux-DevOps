# 🔍 Scenario: Linux Server Audit & Guided Review



## 📝 Concept
Unlike specific "Fix-it" tasks, this scenario focuses on the methodology of auditing a live Linux server to determine its health, purpose, and bottlenecks.

---

## 🛠️ Auditing Checklist

### 1. Service Identification
- **Ports:** `ss -nltp` to identify active services (Web, DB, Cache).
- **Active Apps:** `ps aux` to trace parent-child process relationships.

### 2. Resource Health Check
- **Memory:** `free -h` to check for swap usage (indicating RAM exhaustion).
- **Storage:** `df -h` to ensure no partitions are at 100% capacity.
- **CPU:** `uptime` to analyze load averages over 1, 5, and 15 minutes.

### 3. Log Analysis
- **Journal:** `journalctl -xe` to investigate recent service failures.
- **Auth:** `/var/log/auth.log` to check for failed login attempts.

---

## ✅ Key Takeaways
- Established a systematic approach to server "triage".
- Learned to differentiate between hardware bottlenecks and software misconfigurations.
- Mastered the use of `ss`, `df`, `free`, and `journalctl` for rapid diagnosis.

