---
title: 🖥️ WebUI Management Panel
---

# 🖥️ WebUI Management Panel

Manage your bot through your browser!

## First Time Use

### Get Login Password

When you first start MaiBot, the console will display a password (Token):

```
WebUI Access Token: a1b2c3d4...
Please use this Token to login to WebUI
```

**Important**: This password only shows once, so save it!

### Login Steps

1. Open your browser and visit `http://localhost:8001` (default address)
2. Enter the password shown in the console
3. After successful login, you'll see the management panel

## What Can You Do?

WebUI makes it easy to manage MaiBot:

- ⚙️ **Change Settings** - Modify settings with clicks, no file editing needed
- 🧠 **Manage Memory** - View, edit, and delete bot memories
- 🔌 **Install Plugins** - Install and manage various function plugins
- 📊 **View Statistics** - Check chat records and usage data

## Basic Settings

You can change WebUI settings in `bot_config.toml`:

```toml
[webui]
enabled = true      # Enable WebUI or not
host = "127.0.0.1"  # Bind address
port = 8001         # Port number
```

- Change `host` to `0.0.0.0` to allow access from other devices on the network
- Change `port` to another number to avoid conflicts

## Forgot Your Password?

If you forget your password:

1. Stop MaiBot
2. Delete the `data/webui.json` file
3. Restart MaiBot, and a new password will be generated

## Security Reminders

- Don't share your password with others
- Change the default port when deploying on public networks
- Regular password changes are more secure

## More Features

- [Configuration Management](./config-management.md) - Change settings in browser
- [Memory Management](./memory-management.md) - View and manage memories
- [Plugin Management](./plugin-management.md) - Install and manage plugins
- [Chat Statistics](./chat-stats.md) - View chat statistics