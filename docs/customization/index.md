# Customization Overview

tgfilestream supports modular customization without modifying core logic.

The project is structured to allow:

- External feature injection
- Patch-based enhancements
- Optional components

---

## Architecture Overview

Core

  ↓

Extension Loader

  ↓

Patches

  ↓

Runtime Behavior

---

## Customization Methods

### 1. Configuration-based

Modify `.env` variables to change behavior.

[Environment Variables](../environment-variables/description.md)

### 2. Patch-Based

Inject behavior into specific lifecycle hooks.

---

## Recommended Workflow

1. Do NOT modify core files.
2. Create a patch.
3. Test locally.
4. Deploy.