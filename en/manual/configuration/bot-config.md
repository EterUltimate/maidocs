---
title: Basic Configuration
---

# 🤖 Basic Configuration

The `[bot]` section in `bot_config.toml` is the robot's "ID card", telling MaiBot some basic information.

## 📋 Configuration Items Overview

```toml
[bot]
platform = "qq"           # Platform identifier
qq_account = 0            # QQ account
platforms = []            # Other platform list
nickname = "麦麦"          # Robot nickname
alias_names = []          # Alias list
```

## 🔍 Detailed Explanation of Each Configuration Item

### platform 🏷️ Platform Identifier

**What is this?** Tells MaiBot which chat platform you're using her on.

**How to fill?** Use `"qq"` for QQ, fill in the corresponding identifier for other platforms.

**Example:**
```toml
[bot]
platform = "qq"  # I'm using MaiMai in QQ groups
```

### qq_account 🔢 QQ Account Number

**What is this?** The robot's QQ number, just like your phone number.

**How to fill?** Fill in the numeric ID of the QQ account that the robot logs in with.

**Example:**
```toml
[bot]
qq_account = 123456789  # MaiMai's QQ number is 123456789
```

::: tip 💡 Quick Tip
This number is very important! MaiMai relies on it to identify which messages are @mentions to her.
:::

### platforms 📱 Other Platforms

**What is this?** If MaiMai needs to be used on multiple platforms simultaneously, list other platforms here.

**How to fill?** Fill in a list of platform identifiers, like `["qq", "telegram"]`.

**Example:**
```toml
[bot]
platforms = ["telegram", "discord"]  # Besides QQ, also used on Telegram and Discord
```

::: warning ⚠️ Note
Most users only need QQ, this can be left empty.
:::

### nickname 🏷️ Robot Nickname

**What is this?** What MaiMai is called, how everyone addresses her in groups.

**How to fill?** Call her whatever you want, supports Chinese.

**Example:**
```toml
[bot]
nickname = "XiaoMai"  # Everyone calls her "XiaoMai"
```

### alias_names 🏷️ Alias List

**What is this?** MaiMai's other names, just like you have both a formal name and nicknames.

**How to fill?** Fill in a list, can have many names.

**Example:**
```toml
[bot]
alias_names = ["MaiMai", "XiaoMai", "MaiZi"]  # She responds to any of these names
```

::: tip 💡 Quick Tip
After setting aliases, when someone in the group uses these names to mention MaiMai, she will also reply, just like being @mentioned.
:::

## 🎯 Complete Configuration Example

```toml
[bot]
platform = "qq"                    # Used in QQ groups
qq_account = 123456789             # MaiMai's QQ number
platforms = []                     # Only use QQ, leave other platforms empty
nickname = "MaiMai"                 # Formal name is "MaiMai"
alias_names = ["XiaoMai", "MaiZi"]       # Nicknames are "XiaoMai", "MaiZi"
```

## ⚠️ Important Reminder

The `[bot]` section is MaiBot's **minimum required configuration**, just as important as a person's ID card!

**Two items that must be filled:**
- ✅ `platform` - Tell her which platform
- ✅ `qq_account` - Tell her QQ number

**After filling in basic information, don't forget:**
1. 🧠 Configure [Model](./model-config.md) - Give MaiMai an AI brain
2. 🎭 Configure [Personality](./personality-config.md) - Give MaiMai personality

## 🚀 Next Steps

Basic information configured, next:
- 🆕 **Newbie Recommended**: Go see [Model Config](./model-config.md) to give MaiMai an AI brain
- 🎭 **Want to adjust personality**: Go see [Personality Config](./personality-config.md) to make MaiMai more unique