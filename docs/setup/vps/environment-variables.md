# Environment Variables Configuration

tgfilestream uses environment variables to configure Telegram access, server behavior, performance tuning, and access control.

All variables must be defined inside a `.env` file located in the project root directory.

---

## 1. Create the `.env` File

Inside the project directory:

```bash
nano .env
```

---

## 2. Core & Application Environment Variables
You can find list of required and optional Environment Variables at [environment-variables/description.md](../../environment-variables/description.md)

You can use our [Interactive `.env` Builder](../../environment-variables/env-builder.md) to generate .env file content

??? info "Example `.env` File"
    ```env
    API_ID=123456
    API_HASH=your_api_hash_here
    BOT_TOKEN=123456:ABC-YourBotTokenHere
    BIN_CHANNEL=-1001234567890

    HOST=0.0.0.0
    PORT=8080
    PUBLIC_URL=https://yourdomain.com
    DEBUG=False

    CONNECTION_LIMIT=5
    DOWNLOAD_PART_SIZE=1048576
    NO_UPDATE=False
    SEQUENTIAL_UPDATES=False
    FILE_INDEX_LIMIT=10
    MAX_WARNS=3

    ADMIN_IDS=123456789,987654321
    ALLOWED_IDS=123456789

    MULTI_TOKEN1=1234567890:AAExampleBotTokenGeneratedHere
    MULTI_TOKEN2=0987654321:AAExampleBotTokenGeneratedHere

    DB_BACKEND=mongodb
    MONGODB_URI=mongodb+srv://myDatabaseUser:Password@cluster0.example.mongodb.net/myDatabase?retryWrites=true&w=majority
    MONGODB_DBNAME=TGFS
    ```

---

## 4. Important Notes
- Never commit `.env` to Git.
- Add `.env` to `.gitignore`.
