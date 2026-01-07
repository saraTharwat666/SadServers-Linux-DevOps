# 🐘 Scenario: Helsingør - Postgres Physical Replication

![Docker Container](https://media.tenor.com/soT27Z6i4gMAAAAM/hop-on-docker-desktop-docker.gif)

## 📝 Challenge Description
The goal was to fix a broken **PostgreSQL Physical Replication** setup managed via Docker Compose. While the Master node was functional, the Replica node was failing to synchronize and stay online.

**Constraints:**
- Use `docker compose` to manage services.
- Ensure the Replica container successfully joins the cluster.
- The solution is verified by a health check script.

---

## 🔍 Root Cause Analysis (RCA)
By inspecting the container logs using `docker logs postgres-db-replica`, the following critical error was identified:
> `FATAL: recovery aborted because of insufficient parameter settings`
> `DETAIL: max_connections = 80 is a lower setting than on the primary server (100).`

In PostgreSQL physical replication, certain parameters on the **Replica** must be greater than or equal to the settings on the **Master**. These include:
- `max_connections`
- `max_locks_per_transaction`
- `max_worker_processes`

---

## 🛠️ The Fix

### 1. Identify Master Settings
Checked the current configuration on the Master node:
```bash
docker exec postgres-db-master psql -U helsingor -d postgres -c "show max_connections;"
```



2. Synchronize Configurations
Updated the postgres.conf (or the environment variables in docker-compose.yml) for the Replica to match the Master's capacity:

Set ```max_connections = 100``` (Minimum)

Set ```max_wal_senders = 10```

Set ```max_locks_per_transaction = 64```



3. Deploy Changes
Restarted the stack with the --force-recreate flag to apply the new configuration:

```Bash
docker compose up -d --force-recreate
```

✅ Verification
The fix was confirmed by checking the replication status from the Master:

```Bash
docker exec postgres-db-master psql -U helsingor -c "select * from pg_stat_replication;"
```
























