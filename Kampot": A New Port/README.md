# 🛠️ SadServers Challenge: "Kampot" - Port Forwarding Magic

## 📋 Scenario Overview
A Python application is running on port `20280`. However, an internal legacy monitoring system is hardcoded to look for the service on port `80`. Since the application configuration cannot be modified, we must redirect traffic from port 80 to 20280 locally.

- **Level:** Easy
- **Category:** Networking / IPTables
- **App Port:** 20280
- **Target Port:** 80

---

## 🔍 The Challenge: PREROUTING vs. OUTPUT
Initially, applying a `PREROUTING` rule didn't work for `localhost` requests. This is because:
1. **PREROUTING** handles packets coming from *outside* the system.
2. **OUTPUT** handles packets generated *locally* (like our `curl` command).

To solve this completely, the redirection must be applied to the `OUTPUT` chain for local traffic.

---

## 🛠️ Solution Steps

### 1. Apply Local Port Forwarding
We tell the Kernel to redirect any local TCP traffic destined for port 80 over to port 20280:
```bash
sudo iptables -t nat -A OUTPUT -o lo -p tcp --dport 80 -j REDIRECT --to-port 20280
```
2. Apply External Port Forwarding (Optional but Good Practice)
To ensure the service is also reachable from external sources:

```Bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 20280
```
3. Verification
Testing the connection to ensure the mapping is active:

```Bash
curl localhost:80/accounts
```
# ```Result: [{"id":1,"name":"Alice",...}]```
💡 Key Takeaways
IPTables Tables: Use the nat table for address/port translation.

The Loopback Interface (lo): Local requests travel through the lo interface and bypass the PREROUTING chain.

Networking Layers: Understanding how a packet travels through the Linux Kernel chains is essential for effective debugging.
