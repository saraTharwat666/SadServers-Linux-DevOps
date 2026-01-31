# 🛠️ SadServers Challenge: "Manhattan" - Troubleshooting Postgres Write Failures

## 📋 Scenario Overview
A PostgreSQL database was unable to accept new data (`INSERT` commands). The service was managed by `systemd` and configured to store data in a non-standard directory: `/opt/pgdata`.

- **Level:** Medium
- **Category:** Storage / Database Administration
- **Database:** PostgreSQL 14
- **Mount Point:** `/opt/pgdata`

---

## 🔍 Root Cause Analysis

### 1. Connection Failure
Initially, the `psql` client couldn't connect to the server. This happened because the PostgreSQL service had crashed and couldn't create its Unix socket file in `/var/run/postgresql/`.

### 2. Disk Space Exhaustion
Using `df -h` and `ls -la`, I discovered that the partition `/opt/pgdata` was **100% full**. 
- **The Culprits:** Large backup files (`file1.bk`, `file2.bk`) and a dummy file named `deleteme` were occupying over 8GB of space.
- **The Impact:** PostgreSQL requires free space for Write-Ahead Logging (WAL) and temporary operations. Without space, the engine shuts down to prevent data corruption.

---

## 🛠️ Solution Steps

### 1. Identify the Bottleneck
Checked the disk usage and Inodes:
```bash
df -h /opt/pgdata
df -i /opt/pgdata
```


```
df -i /opt/pgdata
```
2. Cleanup
Removed the unnecessary large backup files to free up space:

```Bash
sudo rm /opt/pgdata/*.bk
sudo rm /opt/pgdata/deleteme
```

3. Service Recovery
Restarted the PostgreSQL service to allow it to re-initialize the socket and data processes:

```Bash
sudo systemctl restart postgresql
```

4. Verification
Executed the test insert command:

```Bash
sudo -u postgres psql -c "insert into persons(name) values ('jane smith');" -d dt
```
# Expected Result: INSERT 0 1
















