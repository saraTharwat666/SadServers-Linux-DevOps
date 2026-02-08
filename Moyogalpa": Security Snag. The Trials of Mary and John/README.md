# SadServer Solution: "Moyogalpa" - Security Snag

## Scenario Overview

The goal was to fix a Go web application named `webapp` that was failing to serve static files over HTTPS on port 7000.

## Technical Issues & Fixes

1. **Hostname Resolution**: Fixed by adding `127.0.0.1 webapp` to `/etc/hosts`.

2. **SSL Trust**: Imported the custom CA certificate (`CA.crt`) into the system trust store using `update-ca-certificates` to allow secure communication.

3. **File Permissions**: Changed ownership of `/home/webapp/pki` and `/home/webapp/static-files` to the `webapp` user and set appropriate read permissions.

4. **AppArmor Restriction**: This was the most critical step. The AppArmor profile for `/usr/local/bin/webapp` was restricting access to static files. I updated the profile located at `/etc/apparmor.d/usr.local.bin.webapp` to allow read access (`r`) to the static files directory.

## Success Criteria
- Running `curl https://webapp:7000/users.html` returns a `200 OK` status with the file content.
- The `check.sh` script returns `OK`.
