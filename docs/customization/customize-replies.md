# Customizing Bot Reply Text

tgfilestream uses a translation registry system to manage reply texts.

All default messages are defined in:

tgfs/utils/translation.py

Messages are loaded using a registry-based system.

This allows overriding text without modifying core files.

---

# How It Works

The core project defines:

- A base language class (`en`)
- A registry dictionary
- Messages fetched dynamically via registry

Example (simplified):

```python
registry = {}
registry["en"] = en
```

When the bot needs a message, it fetches it from the active language class in the registry.

---

# Overriding Reply Text Using a Patch

To override messages, you create a patch that:

1. Inherits from the base language class
2. Overrides specific text variables
3. Registers your custom class into the registry

---

# Official CustomReply Patch

You can use the official template:

https://github.com/SpringsFern/CustomReply

---

# Step-by-Step Guide

## 1. Fork the Repository

Fork:

https://github.com/SpringsFern/CustomReply

Modify the code inside:

```
CustomReply/__init__.py
```

---

## 2. Example Customization

```python
from tgfs.utils.translation import registry, en

class CustomText(en):
    START_TEXT = "Hey there how are you\nYou can send me any file and I will generate a link for that file."

registry["en"] = CustomText
```

This overrides the default `START_TEXT`.

You can override any variable defined in:

https://github.com/SpringsFern/tgfilestream/blob/main/tgfs/utils/translation.py

---

## 3. Install the Patch

Clone your fork inside:

```
<tgfilestream base folder>/tgfs/patches/
```

Example:

```bash
cd tgfs/patches
git clone https://github.com/<your-username>/CustomReply
```

---

## 4. Restart tgfilestream

```bash
sudo systemctl restart tgfs
```

Or if running manually:

```bash
python -m tgfs
```

---

# Adding New Language Support

You can:

1. Submit pull request to:
   https://github.com/SpringsFern/tgfs_translation

OR

2. Add new language class inside your patch:

```python
class CustomSpanish(en):
    START_TEXT = "Hola!"

registry["es"] = CustomSpanish
```

---

# Best Practices

- Do not modify `translation.py` directly.
- Always use patches.
- Only override variables you need.
- Keep your patch repository separate for easier upgrades.
