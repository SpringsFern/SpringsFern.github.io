# Python Environment Setup

This section explains how to create an isolated Python environment and install the required dependencies.

Using a virtual environment ensures that project dependencies do not interfere with system-wide Python packages.

---

## 1. Verify Python Version

tgfilestream requires:

- Python 3.10 or higher

Check your version:

```bash
python3 --version
```

If Python is not installed:

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip -y
```

---

## 2. Create Virtual Environment

Inside the project directory:

```bash
python3 -m venv venv
```

This creates a folder named `venv/` containing an isolated Python environment.

---

## 3. Activate Virtual Environment

```bash
source venv/bin/activate
```

After activation, your terminal should show:

```
(venv)
```

This indicates the environment is active.

---

## 4. Upgrade pip (Recommended)

```bash
pip install --upgrade pip
```

---

## 5. Install Project Dependencies

Install all required packages:

```bash
pip install -r requirements.txt
```

Wait for installation to complete.

---

## 6. Verify Installation

You can verify installed packages:

```bash
pip list
```

Ensure required libraries such as:

- aiohttp
- telethon
- dnspython
- python-dotenv (if used)

are installed successfully.

---

## 7. Deactivate Virtual Environment (Optional)

To exit the virtual environment:

```bash
deactivate
```

You must reactivate it before running the application.

---

## Important Notes

- Always activate the virtual environment before running the application.
- Do not install dependencies globally.
- Do not delete the `venv/` directory after setup.
