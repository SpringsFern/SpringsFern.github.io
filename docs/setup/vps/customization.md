# Customization & Extensibility

At this point, tgfilestream is fully deployed and running in production.

You may now customize behavior, extend features, or integrate additional components.

tgfilestream is designed with extensibility in mind, allowing:

- Patch-based feature injection
- Custom command handling
- Database behavior overrides

---

## Customization Areas

You can customize:

- Bot commands
- Bot reply text
- Access control logic
- Download handling
- Worker behavior

---

## Where to Start

See:

➡ [`/customization/index.md`](../../customization/index.md)

This section explains:

- How customization works internally
- Available extension points
- patch architecture
- Safe modification guidelines

---

⚠ Important:

Avoid modifying core files directly.
Use extension mechanisms when possible to maintain upgrade compatibility.