🛠️ SadServers Challenge: "Lisbon" - The Multi-Layered Nightmare

## 📋 Scenario Overview
The goal was simple: Retrieve the value of the key `foo` from a secured `etcd` server. However, the path was blocked by three different layers of "traps."

- **Level:** Medium/Hard
- **Target Key:** `foo`
- **Expected Result:** `bar`

---

## 🔍 Investigation & Troubleshooting Steps

### Layer 1: The Time Paradox (Expired Certificates)
When attempting to use `etcdctl`, the first error was `x509: certificate has expired`.
- **Finding:** The certificates were valid in 2023, but the system clock was set to 2026/2027.
- **Solution:** Rolled back the system time to a valid window.
```bash
sudo date -s "2023-01-01 00:00:00"
```
Layer 2: The Network Decoy (Iptables Redirection)
Even after fixing the time, connection attempts were hitting an Nginx server (returning 404) instead of etcd.

Finding: A hidden iptables NAT rule was redirecting traffic from port 2379 to 443.

Solution: Flushed the NAT table to allow direct access to the etcd port.

```Bash
sudo iptables -t nat -F
```
Layer 3: The Syntax Trap (API v3 & Argument Order)
The default etcdctl client was defaulting to API v2, and version 3.3.x is extremely sensitive to argument ordering.

Finding: Placing flags after the get command caused "unknown command" errors.

Solution: Explicitly set the API version and followed a strict argument order.

✅ The Final Winning Command
```Bash
ETCDCTL_API=3 etcdctl --endpoints=https://localhost:2379 \
  --cert=/etc/ssl/certs/localhost.crt \
```
  --key=/etc/ssl/certs/localhost.key \
  --cacert=/etc/ssl/certs/localhost.crt \
  --insecure-skip-verify get foo
