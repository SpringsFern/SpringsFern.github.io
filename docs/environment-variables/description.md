# Environment Variables Configuration

tgfilestream uses environment variables to configure Telegram access, server behavior, performance tuning, and access control.

## 1. Core & Application Environment Variables

[Click Here](https://github.com/SpringsFern/tgfilestream/blob/main/README.md#%EF%B8%8F-environment-variables)

---

## 2. Detailed Explanation of Variables

### Telegram Credentials

#### API_ID
App ID of your Telegram application.  
Obtained from https://my.telegram.org under "API Development Tools".

#### API_HASH
App Hash of your Telegram application.  
Obtained from https://my.telegram.org under "API Development Tools".

#### BOT_TOKEN
Token generated via @BotFather.  
Used to authenticate your bot.

#### BIN_CHANNEL
The Telegram channel ID which is used to share files between multiple bots.  
Must be the full numeric ID (usually starts with `-100`).  
The bot must be an admin in this channel.

---

### Server Configuration

#### HOST
IP address the server binds to.  
Use `0.0.0.0` to allow external access.

#### PORT
Port where the application runs.

#### PUBLIC_URL
The public domain used to generate download links.  
When using Nginx + SSL, this should be your domain.

Example:
```
https://yourdomain.com
```

#### DEBUG
Enables verbose logging.  
Set to `False` in production.

---

### Performance Settings

#### CONNECTION_LIMIT
Number of connections created per Telegram Data Center for a single client.  
Higher values increase performance but use more memory.

#### DOWNLOAD_PART_SIZE
Size of each chunk requested from Telegram (in bytes).  
Default is `1048576` (1MB).  
Increasing this may improve speed but increases memory usage.

#### MULTI_TOKENx
Defines multiple Telegram bot tokens.  
Each token runs as a separate client to reduce flood wait errors.

Example:
```
MULTI_TOKEN1=...
MULTI_TOKEN2=...
```

---

### Bot Behavior Settings

#### NO_UPDATE
If set to `True`, the bot will not reply to incoming messages.

#### SEQUENTIAL_UPDATES
If `True`, Telegram updates are processed sequentially instead of concurrently.

#### FILE_INDEX_LIMIT
Maximum number of files shown when using the `/files` command.

#### MAX_WARNS
Number of warnings allowed before banning a user.

#### ADMIN_IDS
Comma-separated Telegram user IDs allowed to use admin commands.

#### ALLOWED_IDS
If set, only these user IDs can interact with the bot.

---

## 4. Important Notes
- Add all MULTI_TOKENx Bots to BIN_CHANNEL
- Never expose `API_HASH`.
- Never share `BOT_TOKEN`.
- Restrict `ADMIN_IDS` carefully.
