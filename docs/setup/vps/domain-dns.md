# Setup Custom Domain & Update PUBLIC_URL

In this step, we will:

1. Point your domain to the VPS
2. Configure Nginx to use the domain
3. Enable HTTPS (SSL)
4. Update `PUBLIC_URL`
5. Restart services

---

## 1. Create DNS A Record

Go to your domain provider (Namecheap, GoDaddy, Cloudflare, etc.)

Create an **A record**:

| Type | Name | Value | TTL |
|------|------|-------|-----|
| A | @ | YOUR_SERVER_IP | Auto |

If using subdomain (recommended):

| Type | Name | Value |
|------|------|-------|
| A | dl | YOUR_SERVER_IP |

Example:

```
dl.yourdomain.com → 123.45.67.89
```

---

## 2. Verify DNS Propagation

On your server:

```bash
ping dl.yourdomain.com
```

Or:

```bash
nslookup dl.yourdomain.com
```

It should resolve to your VPS IP.

DNS propagation may take a few minutes.

---

## 3. Update Nginx Configuration

Edit your site config:

```bash
sudo nano /etc/nginx/sites-available/tgfs
```

Change:

```nginx
server_name _;
```

To:

```nginx
server_name dl.yourdomain.com;
```

Save and test:

```bash
sudo nginx -t
```

Reload:

```bash
sudo systemctl reload nginx
```

Now test:

```
http://dl.yourdomain.com
```

It should work over HTTP.

---

## 4. Enable HTTPS (Let's Encrypt)

Install Certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Run:

```bash
sudo certbot --nginx -d dl.yourdomain.com
```

Certbot will:

- Generate SSL certificate
- Modify Nginx config automatically
- Enable HTTPS
- Set up auto-renewal

Verify renewal:

```bash
sudo certbot renew --dry-run
```

Now test:

```
https://dl.yourdomain.com
```

---

---

### Cloudflare Users (Important)

If your domain is proxied through Cloudflare (orange cloud enabled), you must configure SSL correctly to avoid infinite redirect loops.

#### Set SSL Mode Properly

Go to:

Cloudflare Dashboard → SSL/TLS → Overview

Set SSL mode to:

Full (Strict)  ✅ Recommended

Do NOT use:

Flexible ❌ (This will cause redirect loops)

---

??? info "Why Flexible Mode Causes Infinite Redirect"
    In Flexible mode:

    User → Cloudflare (HTTPS)  
    Cloudflare → Server (HTTP)  

    Your Nginx then redirects HTTP → HTTPS.  
    Cloudflare again connects via HTTP.  

    Result: Infinite redirect loop.

#### Recommended Cloudflare Settings

SSL/TLS → Overview:
- SSL Mode: Full (Strict)

SSL/TLS → Edge Certificates:
- Always Use HTTPS: ON (Optional but recommended)

---

## 5. Update PUBLIC_URL

Open `.env`:

```bash
nano .env
```

Change:

```
PUBLIC_URL=https://0.0.0.0:8080
```

To:

```
PUBLIC_URL=https://dl.yourdomain.com
```

⚠ `PUBLIC_URL` must match your final HTTPS domain.

This ensures:

- Download links are generated correctly
- Users receive valid public URLs

---

## 6. Restart tgfilestream

After updating `.env`, restart services:

```bash
sudo systemctl restart tgfs
```

Restart workers (if any):

```bash
sudo systemctl restart tgfs-worker@8081
sudo systemctl restart tgfs-worker@8082
sudo systemctl restart tgfs-worker@8083
```

---

## Security Recommendations

- Always use HTTPS in production
- Keep `DEBUG=False`
- Do not expose raw worker ports publicly
- Use firewall to block 8080–8084 from external access

Example:

```bash
sudo ufw deny 8080
sudo ufw deny 8081
sudo ufw deny 8082
sudo ufw deny 8083
```
