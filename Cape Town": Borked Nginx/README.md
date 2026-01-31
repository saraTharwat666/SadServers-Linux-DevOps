# 🛠️ SadServers Challenge: "Cape Town" - Fixing a Broken Nginx Server

## 📋 Scenario Overview
The Nginx server was unresponsive, returning `Connection Refused` on port 80. The goal was to restore the default Nginx page.

- **Level:** Medium
- **Category:** Web Servers / System Limits
- **Service:** Nginx

---

## 🔍 Root Cause Analysis

### 1. Configuration Syntax Error
Running `nginx -t` revealed a syntax error in the site configuration:
- **Location:** `/etc/nginx/sites-enabled/default`
- **Error:** An unexpected semicolon `;` was present at the very beginning of the file (Line 1).
- **Impact:** Nginx refused to start as long as any included configuration file had invalid syntax.

### 2. System Resource Limits (The Hidden Trap)
After fixing the syntax, the server returned a `500 Internal Server Error`. The logs (`error.log`) showed: `(24: Too many open files)`.
- **Cause:** The system or service had a very low limit for "File Descriptors" (FDs). 
- **Impact:** Nginx couldn't open the `index.html` file or even create worker processes because it hit the "Open Files" ceiling.

---

## 🛠️ Solution Steps

### Step 1: Fix Syntax
- Edited `/etc/nginx/sites-enabled/default` and removed the rogue `;` from the first line.
- Verified with `sudo nginx -t`.

### Step 2: Raise File Limits
- Modified the service limits using `systemctl edit nginx` to add:
  ```ini
  [Service]
  LimitNOFILE=65535
  ```


Also updated ```/etc/security/limits.conf``` for system-wide persistence.

Step 3: Service Recovery

Reloaded the systemd daemon: ```sudo systemctl daemon-reload```

Restarted Nginx: ```sudo systemctl restart nginx```

Step 4: Verification
Command: ```curl -Is 127.0.0.1:80 | head -1```

Result: HTTP/1.1 200 OK













  
