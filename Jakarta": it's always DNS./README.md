# SadServers Solution: "Jakarta" (It's always DNS) 🇮🇩🌐

## 🔍 Problem
Unable to resolve `google.com`. The `ping` command returned `Name or service not known`, even though the network had a valid default gateway.

## 🛠️ Diagnostics & Findings
1. **Network Check:** `ping 8.8.8.8` failed (100% packet loss), suggesting external ICMP/DNS traffic is blocked.
2. **DNS Check:** `dig @10.1.0.2 google.com` worked! This proved that the internal AWS DNS was reachable and functional.
3. **System Configuration:** Investigated `/etc/nsswitch.conf` and found that the `hosts` line was missing the `dns` entry.

## 🚀 The Fix
1. **Modify Name Service Switch:** Updated `/etc/nsswitch.conf` to allow the system to use DNS:
   ```text
   hosts: files dns
   ```


Set Nameserver: Configured /etc/resolv.conf to use the working internal resolver:

Plaintext
```
nameserver 10.1.0.2
```
Fix Local Hostname: Added the local hostname to /etc/hosts to resolve sudo slowness.

✅ Verification
Running ```ping -c 1 google.com``` successfully resolved to an IP address, confirming the Name Service Switch is correctly routing queries to the DNS resolver.
