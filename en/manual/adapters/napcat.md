---
title: 💬 QQ Bot Connection Guide
---

# 💬 QQ Bot Connection Guide

Want MaiBot to chat with you in QQ groups? NapCat makes it super easy!

## What is NapCat? 🤔

NapCat is like a "translator" that helps MaiBot and QQ chat:
- ✅ No need to install full QQ client
- ✅ Stable, fast, and resource-efficient  
- ✅ Supports group and private chats
- ✅ Auto-connects, no hassle

Simply put: NapCat lets MaiBot "understand" QQ messages and "speak" back to QQ!

## How It Works 🔄

As simple as making a phone call:
1. **NapCat** logs into your QQ number (like you logging into QQ on phone)
2. **MaiBot** starts and waits for connection (like answering a call)
3. **NapCat** automatically finds MaiBot and connects (like dialing connects the call)
4. **Message flow** QQ Message → NapCat → MaiBot → Reply → NapCat → QQ

## Configure MaiBot 🛠️

### Step 1: Tell MaiBot You Want to Use QQ

Add this to `config/bot_config.toml`:

```toml
[bot]
platform = "qq"           # Use QQ platform
qq_account = 123456789    # Your bot's QQ number
nickname = "MaiMai"       # Bot nickname
```

That's it! 🎉

### Step 2: Set Connection Parameters

Still in `config/bot_config.toml`:

```toml
[maim_message]
ws_server_host = "127.0.0.1"   # Server address (use this for local)
ws_server_port = 8080           # Port number (default 8080)
auth_token = []                 # Auth token, leave empty
```

| Setting | What It Means | How to Fill |
|--------|---------------|-------------|
| `ws_server_host` | Server address | Use `127.0.0.1` locally, actual IP for servers |
| `ws_server_port` | Port number | Default `8080`, remember if you change it |
| `auth_token` | Password verification | Leave empty, don't worry about it |

## Configure NapCat 🐱

### Install NapCat

Two ways, pick either:

**Method 1: Docker (Recommended)**

If you use the project's built-in `docker-compose.yml`, NapCat is already included as the `napcat` service and can be started together with MaiBot:

```bash
docker compose up -d
```

The service configuration looks like this:

```yaml
napcat:
  environment:
    - NAPCAT_UID=1000
    - NAPCAT_GID=1000
    - TZ=Asia/Shanghai
  ports:
    - "6099:6099"
  volumes:
    - ./docker-config/napcat:/app/napcat/config
    - ./data/qq:/app/.config/QQ
  container_name: maim-bot-napcat
  restart: always
  image: mlikiowa/napcat-docker:latest
```

After startup, the NapCat WebUI is exposed on port `6099` by default:

- Local access: `http://localhost:6099`
- Config directory: `./docker-config/napcat/`
- QQ login data: `./data/qq/`
- View logs: `docker compose logs -f napcat`

**Method 2: Local Installation**
Download the appropriate version from [NapCatQQ Release](https://github.com/NapNeko/NapCatQQ/releases)

### Set Up Connection

1. Open NapCat's web interface
2. Find "Reverse WebSocket" settings
3. Fill in MaiBot address: `ws://127.0.0.1:8080/ws`

If NapCat and MaiBot both run in Docker Compose, make sure MaiBot's `maim_message.ws_server_host` listens on an address reachable from the container network. If unsure, prefer the project's built-in Docker configuration.

Example config:
```json
{
  "ws": {
    "enable": false
  },
  "reverse-ws": {
    "enable": true,
    "urls": ["ws://127.0.0.1:8080/ws"]
  }
}
```

### Login to QQ

After starting NapCat, you need to login:

1. **QR Code Login**: Look for QR code in logs, scan with phone QQ
2. **Password Login**: Fill account password in config

⚠️ **Important Reminders**:
- Recommend using secondary account to reduce ban risk
- Login info is saved, no need to re-login after restart
- Follow QQ rules, don't spam

## Connection Steps 📋

### Recommended Startup Order:

1. **Start NapCat** → Wait for QQ login success
2. **Start MaiBot** → Wait for WebSocket service startup  
3. **Auto-connect** → NapCat automatically connects to MaiBot

```bash
# Docker one-click startup (recommended)
docker compose up -d

# Manual startup
# Terminal 1: Start NapCat
# Terminal 2: uv run python bot.py
```

### Verify Connection ✅

How to know it's connected? Check these:

1. **MaiBot logs**: See "WebSocket service started successfully"
2. **NapCat logs**: See "Reverse WebSocket connection successful"
3. **Test message**: @bot in QQ group, see if it replies

## Common Issues 🤔

### Can't Connect?

**Check these**:
- Are address and port correct?
- Is firewall blocking?
- Are NapCat and MaiBot on same machine?
- Any error messages in logs?

### Not Receiving Messages?

**Possible causes**:
- Wrong QQ number? Must match NapCat login
- Is NapCat itself receiving messages? Check NapCat logs
- Network connection OK?

### Can't Send Messages?

**Troubleshooting**:
- Does bot have speaking permissions? (Need permissions in groups)
- Any errors in MaiBot logs?
- Multiple programs using same QQ number?

### Other Issues

- **Version compatibility**: Latest NapCat usually works fine
- **Network issues**: Check firewall and network settings
- **Permission issues**: Ensure bot has necessary permissions

### How to Update NapCat?

For Docker deployment, update with:

```bash
docker compose pull napcat
docker compose up -d napcat
```

## Security Reminders 🔒

- Don't expose WebSocket port to public internet
- Using `127.0.0.1` is safer
- Regularly update NapCat version
- Pay attention to QQ usage rules

## Getting Help 💬

If you encounter problems:
- Check NapCat and MaiBot logs
- Join MaiBot community groups to ask
- Submit issues on GitHub
- Search for related tutorials

Good luck with your connection, let MaiBot chat with you in QQ! 🎉
