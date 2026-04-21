---
title: Configuration Overview
---

# 🎯 Configuration Overview

Configuration is telling the robot "who you are", "how to talk", and "what AI brain to use". It's as simple as introducing yourself to a new friend!

## 📋 Configuration File List

MaiBot has two main configuration files, both in the `config/` folder:

| 📁 File | 📝 Purpose | 🔗 Details |
|---------|------------|------------|
| `bot_config.toml` | Basic info, personality, chat habits | [View Details](./bot-config.md) |
| `model_config.toml` | AI model settings (which AI, how to use) | [View Details](./model-config.md) |

::: tip 💡 Quick Tip
When you first start, if MaiBot can't find the configuration files, it will automatically generate default configs. No worries!
:::

## 🧩 What's in bot_config.toml?

This file controls the robot's "personality" and includes these sections:

| 🏷️ Config Section | 🎯 Controls What | 💡 Simple Understanding |
|-----------|------------|------------|
| `[bot]` | Basic info | "What's my name, what's my QQ number" |
| `[personality]` | Personality settings | "Am I lively or quiet? What's my speaking style?" |
| `[chat]` | Chat behavior | "How much do I talk? When do I reply?" |
| `[visual]` | Image capabilities | "Can I understand images? How?" |
| `[memory]` | Memory system | "Should I remember chat content? How much?" |
| `[message_receive]` | Message filtering | "Which messages do I ignore?" |

## ⚡ Hot Reload - Changes Take Effect Immediately!

MaiBot supports **hot reload**, which means:
- ✅ After modifying config files, **no restart** needed
- ✅ After saving files, **automatic reload** of new config
- ✅ **Immediate effect**, see results right away

::: warning ⚠️ Note
Some configs (like changing AI models) may require reinitializing related services to take full effect.
:::

## 🌐 WebUI Graphical Interface

If you don't like manually editing files, MaiBot also provides a web-based configuration interface:

- 🌐 Default address: `http://127.0.0.1:8001`
- 🖱️ Click to change configs with mouse
- 📱 Works on both phone and computer

::: warning 🔒 Security Tip
When using on public networks, it's recommended to use Nginx or other reverse proxy. Don't directly expose port 8001!
:::

## 🔄 Configuration File Version Management

The config file has an `[inner]` section, where the `version` field indicates the file version.

When MaiBot upgrades:
1. 🔍 Automatically detects version differences
2. 💾 Backs up old config to `config/old/` folder
3. 🆕 Generates new config files
4. 🚪 Program exits, requires manual restart

This way you can enjoy new features without losing your original configuration!

## 🚀 What to Read Next?

- 🆕 **Newbie Recommended**: Start with [Basic Config](./bot-config.md) → [Model Config](./model-config.md)
- 🎭 **Want to adjust personality**: See [Personality Config](./personality-config.md)
- 💬 **Want to adjust chatting**: See [Chat Config](./chat-config.md)
- 🧠 **Want to enable memory**: See [Memory Config](./memory-config.md)