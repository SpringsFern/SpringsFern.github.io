# Overview

tgfilestream (TGFS) is a Telegram-based file streaming backend.

It allows users to:

- Send files to a Telegram bot
- Generate public download links
- Stream large files efficiently

---

## Core Purpose

TGFS is designed to:

- Handle large file streaming
- Scale using multiple workers
- Integrate with reverse proxies
- Operate behind Cloudflare
- Support modular customization

---

## Key Features

- Multi-worker architecture
- systemd production deployment
- Nginx load balancing
- Cloudflare compatibility
- Registry-based translation system
- Patch-based extensibility
- MongoDB / MySQL support

---

<!-- ## Why TGFS?

Unlike simple Telegram bots, TGFS is built as:

A scalable backend service.

It separates:

- Telegram client logic
- HTTP streaming layer
- Reverse proxy handling
- Customization via patches

--- -->

## Intended Deployment

TGFS is recommended to run on:

- Linux VPS
- Dedicated server
- Cloud VM

It is not intended for shared hosting environments.
