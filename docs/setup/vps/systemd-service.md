# Setup systemd Service (Production Deployment)

This section explains how to configure tgfilestream using systemd for:

- Automatic startup on boot
- Automatic restart on crash
- Proper process isolation

This guide follows the recommended production setup.

---

## 1. Create Main Service File

Create the service file:

```bash
sudo nano /etc/systemd/system/tgfs.service
```

Paste the following:

```ini
[Unit]
Description=tgfilestream
After=network.target

[Service]
Type=simple
User=<username>
Group=<group>

WorkingDirectory=<tgfilestream Clone Path>

ExecStart=<tgfilestream Clone Path>/<virtual-env-dir>/bin/python -m tgfs --env .env tgfilestream.env

Restart=always
RestartSec=3

NoNewPrivileges=true
PrivateTmp=true

StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

Adjust:

- Replace `<username>` with your system username.
- Replace `<group>` with your group name (often same as username).
- Replace `<tgfilestream Clone Path>` with the absolute path to your cloned tgfilestream directory.
- Replace `<virtual-env-dir>` with the name of your virtual environment directory (usually `venv`).

---

??? info "Example"
    ```ini
    [Unit]
    Description=tgfilestream
    After=network.target

    [Service]
    Type=simple
    User=ubuntu
    Group=ubuntu

    WorkingDirectory=/home/ubuntu/tgfilestream

    ExecStart=/home/ubuntu/tgfilestream/venv/bin/python -m tgfs  --env .env tgfilestream.env

    Restart=always
    RestartSec=3

    NoNewPrivileges=true
    PrivateTmp=true

    StandardOutput=journal
    StandardError=journal

    [Install]
    WantedBy=multi-user.target
    ```

---

## 2. Reload systemd

```bash
sudo systemctl daemon-reload
```

---

## 3. Enable Service

Enable the service to start on boot:

```bash
sudo systemctl enable tgfs
```

---

## 4. Start Service

```bash
sudo systemctl start tgfs
```

---

## 5. Check Status

```bash
sudo systemctl status tgfs
```

The service should show as:

```
Active: active (running)
```

---

## 6. View Logs

```bash
sudo journalctl -u tgfs -f
```

---

## Restart & Stop Commands

Restart:

```bash
sudo systemctl restart tgfs
```

Stop:

```bash
sudo systemctl stop tgfs
```