# Official Patches

This page lists patches officially maintained for tgfilestream.

Patches extend functionality without modifying core files.

---

## Available Patches
### 1. Custom Reply Templates
[GitHub](https://github.com/SpringsFern/CustomReply)

Allows overriding default bot reply messages.
See:
➡ [/customization/customize-replies.md](./customize-replies.md)

### 2. WebPlayer
[GitHub](https://github.com/SpringsFern/tgfs_webplayer)

Simple HTML5 Player

### 3. Translation
[GitHub](https://github.com/SpringsFern/tgfs_translation)

Translation of Bot Reply text to additional languages

### 4. MySQL
[GitHub](https://github.com/SpringsFern/tgfs-mysql)

Use MySQL as Database instead of mongodb

## How Patches Work

Patches are loaded during application startup.

They:

- Hook into lifecycle events
- Override default behavior
- Extend functionality

---

⚠ Always verify compatibility before upgrading core version.