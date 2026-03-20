# Clone the Repository

This section explains how to download the tgfilestream source code to your server.

---

## 1. Navigate to Your Deployment Directory

Choose a suitable directory to store the project.

Example:

```bash
cd $HOME
```

You may also use:

- `/var/www`
- `/opt`
- Any custom deployment directory

---

## 2. Clone the Repository

```bash
git clone https://github.com/SpringsFern/tgfilestream.git
```

---

## 3. Change Directory

```bash
cd tgfilestream
```

Verify the contents:

```bash
ls -la
```

You should see files and directory like:

- main application tgfs
- requirements.txt
- README.md

---

## 4. Optional: Checkout Specific Branch

If deploying a specific branch:

```bash
git checkout <branch-name>
```

Example:

```bash
git checkout simple
```

---

## 5. Keep Repository Updated (Optional)

To update later:

```bash
git pull
```

⚠ If you modified local files, commit or stash changes before pulling updates.
