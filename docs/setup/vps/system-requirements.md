# System Requirements

Before deploying **tgfilestream**, ensure your server meets the following requirements.

---

## Operating System

Recommended:

- Ubuntu 22.04 LTS
- Debian 11+
- Any modern Linux distribution

⚠ Windows is not recommended for production deployment.

---

## Required Software

Make sure the following tools are installed:

### 1. Python

- Python 3.10 or higher

Check version:

```bash
python3 --version
```

If not installed:

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip -y
```

---

### 2. Git

Used to clone the repository.

Check:

```bash
git --version
```

Install (if not installed):

```bash
sudo apt install git -y
```

---

### 3. Nginx (Required for Production)

Used as reverse proxy and load balancer.

Install:

```bash
sudo apt install nginx -y
```

---

## Network Requirements

Make sure the following ports are open:

- 80 (HTTP)
- 443 (HTTPS)
- Application port (if running without Nginx)

If using UFW firewall:

```bash
sudo ufw allow 80
sudo ufw allow 443
```

Enable firewall (if not enabled):

```bash
sudo ufw enable
```

---

## Hardware Requirements

### Minimum

- 1 CPU Core
- 1 GB RAM

### Recommended (For Multiple Workers)

- 2+ CPU Cores
- 2+ GB RAM

More workers require additional memory.

---

## Domain (Optional but Recommended)

For production deployment:

- A domain name
- DNS access to create an A record
- Ability to configure SSL (Let's Encrypt)

---

## Recommended Knowledge

Basic familiarity with:

- Linux terminal
- systemd services
- Nginx configuration
- Environment variables