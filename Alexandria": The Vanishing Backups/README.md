
# 🛠️ SadServers Challenge: "Alexandria" - The Vanishing Backups

## 📋 Scenario Description
A critical automated backup system stopped working 3 days ago without any error notifications. The goal was to identify the root cause, fix the script execution, and ensure the cron job is correctly configured.

- **Difficulty:** Easy
- **Category:** Troubleshooting / System Administration
- **Script Path:** `/opt/backup/backup.sh`
- **Backup Destination:** `/var/backups/daily/`

---

## 🔍 Investigation & Root Cause Analysis

### 1. The Stale Lock File (The Deadlock) 🔒
The backup script uses a **Lock File** mechanism to prevent multiple instances from running simultaneously. 
- **The Issue:** A previous execution failed or was interrupted, leaving a "stale" lock file at `/opt/backup/backup.lock`. 
- **The Result:** Every time the script started, it saw the lock file, assumed another backup was already running, and exited immediately.

### 2. Incorrect Cron Configuration ⏰
Upon inspecting the crontab, two major issues were found:
- **Wrong Path:** The cron job was pointing to an old, non-functional script (`old_backup.sh`) instead of the active one (`backup.sh`).
- **Silent Failures:** The job was redirecting all output (including errors) to `/dev/null`, making the failure invisible to logs.

---

## 🛠️ Solution Steps

### Step 1: Manual Fix
I manually removed the stale lock file to "unlock" the script:
```bash
sudo rm -f /opt/backup/backup.lock
Step 2: Verification
Tested the script manually to ensure it successfully creates a compressed archive:
```


```Bash
sudo bash /opt/backup/backup.sh
```
# Output: Backup successful: 
```/var/backups/daily/backup_20260130_171945.tar.gz```

Step 3: Automating the Fix (Cron Update)
Updated the root crontab using ```sudo crontab -e ``` to point to the correct script and enable logging for future troubleshooting:

Bash
# Corrected Cron Job (Runs every 5 minutes for validation)
```
*/5 * * * * /opt/backup/backup.sh
```
