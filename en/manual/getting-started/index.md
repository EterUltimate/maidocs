---
title: Quick Start
---

# 🚀 MaiBot 5-Minute Quick Start

**Welcome to the world of MaiBot!** Follow this guide and you'll have your bot running in 5 minutes.

## 📋 What do you need to prepare?

Just like making a simple dish, you need 2 ingredients:

### 1️⃣ A QQ Side Account 🆔
- **Must be a QQ side account** (important! Bot accounts risk being banned)
- **Can log in normally** (test it on your phone first)
- **Remember the QQ number and password**

### 2️⃣ An AI Model API Key 🔑

An API key is like a key that lets the bot think and talk.

**Recommended options:**

| Provider | Price | Features | How to Get |
|--------|------|------|----------|
| **OpenAI** | $$$ | Smartest | [Official Website](https://platform.openai.com/) |
| **DeepSeek** | ¥ | Good Chinese, cheap | [Official Website](https://platform.deepseek.com/) |
| **Silicon Flow** | ¥ | Stable, many options | [Official Website](https://cloud.siliconflow.cn/) |

::: tip 💡 Beginner Recommendation
Start with **DeepSeek** or **Silicon Flow** - they're cheap and work great! You get free credits when you register.
:::

**Your API key will look like this:**
```
sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## 🏃‍♂️ Three Steps to Start Your Bot

### Step 1: Download MaiBot 📥

Open your terminal (command line) and type:

```bash
git clone https://github.com/MaiM-with-u/MaiBot.git
cd MaiBot
```

### Step 2: Install Dependencies 🔧

```bash
# Install uv (like installing a new tool for your computer)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install MaiBot components
uv sync
```

::: tip 💡 Windows Users
Use PowerShell to run:
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
uv sync
```
:::

### Step 3: Configure Your Bot ⚙️

#### 1. Set up QQ Account

Open the file `config/bot_config.toml`, find this line:
```toml
qq_account = 123456789  # Change to your bot's QQ number
```

#### 2. Set up AI Model

Open the file `config/model_config.toml`, find:
```toml
api_key = "sk-xxxxxxxx"  # Change to your API key
base_url = "https://api.deepseek.com"  # Change to your provider's address
```

### Step 4: Launch! 🚀

```bash
uv run python bot.py
```

The first launch will ask if you agree to the user agreement, type: **agree**

## 📱 Connect to QQ

### Method 1: Use NapCat (Recommended)

1. **Install NapCat** (QQ connector):
```bash
# Download NapCat
git clone https://github.com/NapNeko/NapCatQQ.git
cd NapCatQQ

# Install dependencies
npm install

# Start NapCat
npm run start
```

2. **Scan QR code**: NapCat will show a QR code, scan it with your **QQ side account** to log in

3. **Configure connection**: In `config/bot_config.toml` confirm:
```toml
platform = "qq"
qq_account = your_qq_number
```

### Method 2: Use Docker (Simpler)

If you know Docker, just:
```bash
docker compose up -d
```

Docker will automatically install NapCat and connect everything for you.

## 🎉 Start Chatting!

### Test Your Bot

1. **In a QQ group @ the bot**, say:
```
@MaiMai Hello!
```

2. **Or private message the bot**, say:
```
Hello, I'm your new friend!
```

### Success Indicator ✅

If the bot replies to you, congratulations! 🎉

```
Hello! I'm MaiMai, nice to meet you~
```

## 🆘 Common Problems (Don't Panic)

### Bot Not Responding?

Check these things:
- ✅ Did QQ login succeed?
- ✅ Did you enter the API key correctly?
- ✅ Did you save the configuration files?
- ✅ Check the logs for any errors?

### QR Code Login Failed?

- Restart NapCat and try again
- Check network connection
- Make sure it's a QQ side account, not your main account

### Error "API key invalid"?

- Re-copy the API key, don't copy extra spaces
- Check if the provider address is correct
- Confirm your account still has balance

### Don't Know Command Line?

- Windows users can use **Git Bash**
- Or just download **Docker Desktop** and use the graphical interface

## 🎯 What to Do Next?

Congratulations on successfully starting MaiBot! Now you can:

- 📚 **Read configuration docs** to make your bot smarter
- 🔌 **Install plugins** to add new features  
- 💬 **Add bot to groups** and play with everyone
- 🎨 **Customize settings** to make the bot more like you

Remember: **The bot is alive**, it will slowly learn and become more understanding of you!

---

**Having problems?** Join the group and ask everyone: [MaiMai EEG](https://qm.qq.com/q/RzmCiRtHEW)

**Have fun!** 🌟