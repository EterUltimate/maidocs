---
title: Deployment Overview
---

# 🚀 MaiBot Deployment Overview

**MaiBot is a smart chatbot** that can chat with you, tell jokes, and answer questions. You just need to prepare two things to get it running:

📱 **A QQ side account** (for logging in the bot)
🔑 **An AI model API key** (like a key that lets the bot think and talk)

## 📦 Choose Your Deployment Method

MaiBot offers 2 installation methods, choose any one:

| Method | Suitable For | Difficulty |
|------|---------|------|
| [Source Installation](./installation.md) | Users who want to tinker and control details | ⭐⭐ |
| [Docker Deployment](./docker.md) | One-click deployment, server users | ⭐ |

::: tip 💡 Beginner Recommendation
First time using? Follow this order:

1. **Install MaiBot first** → [Installation Guide](./installation.md)
2. **Then connect to QQ** → [NapCat Adapter](../adapters/napcat.md)
3. **Finally configure AI model** → [Model Configuration](../configuration/model-config.md)
:::

## ⚡ 3-Minute Quick Start

If you already have Python environment and AI API ready, you can start right away:

```bash
# 1. Download MaiBot
git clone https://github.com/MaiM-with-u/MaiBot.git
cd MaiBot

# 2. Install dependencies (like installing parts for the robot)
uv sync

# 3. Launch!
uv run python bot.py
```

The first startup will ask you to agree to the user agreement, just type "agree". That's it!
