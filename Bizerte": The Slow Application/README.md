1. Modified the Systemd service file to point to the correct Redis host:
   ```bash
   sudo sed -i 's/REDIS_HOST=127.0.0.2/REDIS_HOST=127.0.0.1/' /etc/systemd/system/slow-app.service
   ```
Reloaded the systemd daemon and restarted the service:

```Bash
sudo systemctl daemon-reload
sudo systemctl restart slow-app.service
```
Results
Response time dropped from >5 seconds to milliseconds.

```curl localhost:5000``` successfully returns: "Data from FAST cache!"
