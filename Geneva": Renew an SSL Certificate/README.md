1. Discovery & Path Identification
The first step was to locate the certificate and private key files currently used by Nginx:

```
sudo grep -r "ssl_certificate" /etc/nginx/
```
Identified Paths:

Certificate: /etc/nginx/ssl/nginx.crt

Private Key: /etc/nginx/ssl/nginx.key

2. Metadata Extraction
To ensure the new certificate matched the old one, I extracted the Subject information using openssl:
```
openssl x509 -in /etc/nginx/ssl/nginx.crt -noout -subject
```

Extracted Subject Data:
```
CN=localhost, O=Acme, OU=IT Department, L=Geneva, ST=Geneva, C=CH
```

3. Implementation (Certificate Generation)
Using the extracted data, I generated a new self-signed certificate valid for 365 days, overwriting the expired files:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/nginx/ssl/nginx.key \
-out /etc/nginx/ssl/nginx.crt \
-subj "/C=CH/ST=Geneva/L=Geneva/O=Acme/OU=IT Department/CN=localhost"
```
4. Activation & Final Testing
After updating the files, I reloaded the Nginx service and verified the new expiration dates:

# Reload Nginx configuration
```
sudo systemctl reload nginx
```

# Verify the new certificate dates and subject
```
echo | openssl s_client -connect localhost:443 2>/dev/null | openssl x509 -noout -dates -subject
```
