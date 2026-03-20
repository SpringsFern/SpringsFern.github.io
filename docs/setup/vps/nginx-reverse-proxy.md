# Setup Nginx Reverse Proxy (IP-Based Setup)

In this step, we configure Nginx to:

- Reverse proxy requests to tgfilestream
- Handle HTTP traffic
- Properly stream large files

⚠ At this stage, we are NOT using a domain.
We will configure Nginx using the server IP address.

---

## 1. Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
```

Enable and start Nginx:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

---

## 2. Ensure TGFS Service Is Running

Make sure the main service is active:

```bash
sudo systemctl status tgfs
```

The application should be running on:

- `<Bind Address>:<PORT>`

!!! info "Example"
    ```
    127.0.0.1:8080
    ```

---

## 3. Create Nginx Configuration

Create a new site configuration:

```bash
sudo nano /etc/nginx/sites-available/tgfs
```

Paste:

```nginx
upstream aiohttp_backend {
    least_conn;
    keepalive 32;

    server <Bind Address>:<PORT> max_fails=3 fail_timeout=30s;
}

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://aiohttp_backend;

        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_redirect off;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Adjust:

- Replace `<Bind Address>` with the `HOST` value from your `.env`.
- Replace `<PORT>` with the `PORT` value from your `.env`.

---

??? info "Example"
    ```nginx
    upstream aiohttp_backend {
        least_conn;
        keepalive 32;

        server 127.0.0.1:8080 max_fails=3 fail_timeout=30s;
    }

    server {
        listen 80;
        server_name _;

        location / {
            proxy_pass http://aiohttp_backend;

            proxy_http_version 1.1;
            proxy_buffering off;
            proxy_redirect off;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
    ```

---

## 4. Enable Site

```bash
sudo ln -s /etc/nginx/sites-available/tgfs /etc/nginx/sites-enabled/
```

Remove the default site (recommended):

```bash
sudo rm /etc/nginx/sites-enabled/default
```

---

## 5. Test Configuration

```bash
sudo nginx -t
```

If successful:

```
syntax is ok
test is successful
```

Reload Nginx:

```bash
sudo systemctl reload nginx
```

---

## 6. Test Using Server IP

Get your server IP:

```bash
curl ifconfig.me
```

Open in browser:

```
http://<your-server-ip>
```

Or test using curl:

```bash
curl http://<your-server-ip>
```

If configured correctly, requests should be forwarded to TGFS.

---

## Troubleshooting

### 502 Bad Gateway

Check TGFS status:

```bash
sudo systemctl status tgfs
```

Check logs:

```bash
sudo journalctl -u tgfs -f
```

### Nginx Errors

```bash
sudo nginx -t
sudo journalctl -u nginx -f
```
