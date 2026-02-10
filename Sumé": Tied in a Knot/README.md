# SadServers Solution: "The DNS Knot" 🐧🌐

This repository contains the solution for the **"The DNS Knot"** challenge from SadServers. The task involved troubleshooting and fixing a broken DNS configuration in a Linux environment using Knot DNS.

## 🔍 Problem Description
The system was unable to resolve or connect to `blog.sadservers.internal` and `api.sadservers.internal`. 

### Identified Issues:
1.  **Syntax Errors:** The zone file used `CNAM` instead of the correct `CNAME` record type.
2.  **FQDN Issues:** Missing trailing dots (`.`) in the CNAME target, causing incorrect resolution.
3.  **Wrong IPs:** The `A records` were pointing to incorrect or unreachable IP addresses.
4.  **Resolver Conflict:** The system was prioritizing `/etc/hosts` or incorrect nameservers over the local Knot DNS.

## 🛠️ The Solution

### 1. Fix the DNS Zone File
Edited `/var/lib/knot/zones/sadservers.internal.zone`:
- Corrected record type to `CNAME`.
- Added the trailing dot: `www.sadservers.internal.`
- Updated IPs to the correct Docker container IPs:
  - `www` -> `203.0.113.10`
  - `api` -> `203.0.113.20`
- Incremented the `Serial` number.

### 2. Apply Changes
Executed the following commands to reload the DNS server:
```bash
sudo knotc zone-check sadservers.internal
sudo knotc zone-reload sadservers.internal
```
3. Update System Resolver
Modified /etc/resolv.conf to ensure the system queries the local DNS server first:

Plaintext
```
nameserver 127.0.0.1
```
✅ Verification
Successful connection to both services:

```curl blog.sadservers.internal``` -> Success!

```curl api.sadservers.internal ```-> Success!
