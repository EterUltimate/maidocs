---
title: 快速上手
---

# 🚀 MaiBot 5 分钟快速上手

**欢迎来到 MaiBot 的世界！** 跟着这个指南，5 分钟就能让你的机器人跑起来。

## 📋 你需要准备什么？

就像做一道简单的菜，你需要 2 样食材：

### 1️⃣ 一个 QQ 小号 🆔
- **必须是 QQ 小号**（重要！机器人账号有被封风险）
- **能正常登录**（最好先在手机上试试）
- **记住 QQ 号和密码**

### 2️⃣ 一个 AI 模型的 API 密钥 🔑

API 密钥就像一把钥匙，让机器人能思考说话。

**推荐几个选择：**

| 服务商 | 价格 | 特点 | 获取方式 |
|--------|------|------|----------|
**OpenAI** | $$$ | 最聪明 | [官网注册](https://platform.openai.com/) |
| **DeepSeek** | ¥ | 中文好，便宜 | [官网注册](https://platform.deepseek.com/) |
| **硅基流动** | ¥ | 稳定，选择多 | [官网注册](https://cloud.siliconflow.cn/) |

::: tip 💡 新手推荐
先用 **DeepSeek** 或 **硅基流动**，便宜又好用！注册就送免费额度。
:::

**拿到 API 密钥后长这样：**
```
sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## 🏃‍♂️ 三步启动机器人

### 第一步：下载 MaiBot 📥

打开终端（命令行），输入：

```bash
git clone https://github.com/MaiM-with-u/MaiBot.git
cd MaiBot
```

### 第二步：安装依赖 🔧

```bash
# 安装 uv（就像给电脑装个新工具）
curl -LsSf https://astral.sh/uv/install.sh | sh

# 安装 MaiBot 需要的组件
uv sync
```

::: tip 💡 Windows 用户
用 PowerShell 运行：
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
uv sync
```
:::

### 第三步：配置机器人 ⚙️

#### 1. 设置 QQ 账号

打开文件 `config/bot_config.toml`，找到这一行：
```toml
qq_account = 123456789  # 改成你的机器人 QQ 号
```

#### 2. 设置 AI 模型

打开文件 `config/model_config.toml`，找到：
```toml
api_key = "sk-xxxxxxxx"  # 改成你的 API 密钥
base_url = "https://api.deepseek.com"  # 改成你的服务商地址
```

### 第四步：启动！🚀

```bash
uv run python bot.py
```

第一次启动会问你是否同意用户协议，输入：**同意**

## 📱 连接 QQ

### 方法 1：用 NapCat（推荐）

1. **安装 NapCat**（QQ 连接器）：
```bash
# 下载 NapCat
git clone https://github.com/NapNeko/NapCatQQ.git
cd NapCatQQ

# 安装依赖
npm install

# 启动 NapCat
npm run start
```

2. **扫码登录**：NapCat 会显示一个二维码，用你的 **QQ 小号** 扫码登录

3. **配置连接**：在 `config/bot_config.toml` 里确认：
```toml
platform = "qq"
qq_account = 你的QQ号
```

### 方法 2：用 Docker（更简单）

如果你会用 Docker，直接：
```bash
docker compose up -d
```

Docker 会自动帮你装好 NapCat 并连接好。

## 🎉 开始聊天！

### 测试机器人

1. **在 QQ 群里 @机器人**，说：
```
@麦麦 你好！
```

2. **或者私聊机器人**，说：
```
你好，我是你的新朋友！
```

### 成功标志 ✅

如果机器人回复你了，恭喜你！🎉

```
你好呀！我是麦麦，很高兴认识你～
```

## 🆘 常见问题（别慌）

### 机器人不回复？

检查这几件事：
- ✅ QQ 登录成功了吗？
- ✅ API 密钥填对了吗？
- ✅ 配置文件保存了吗？
- ✅ 看日志有什么报错？

### 扫码登录失败？

- 重启 NapCat 再试一次
- 检查网络连接
- 确认是 QQ 小号，不是主号

### 报错说 "API key invalid"？

- 重新复制 API 密钥，别多复制空格
- 检查服务商地址对不对
- 确认账户里还有余额

### 不会用命令行？

- Windows 可以用 **Git Bash**
- 或者直接下载 **Docker Desktop**，用图形界面

## 🎯 下一步做什么？

恭喜你成功启动了 MaiBot！现在你可以：

- 📚 **看配置文档**，让机器人更聪明
- 🔌 **装插件**，给机器人加新功能  
- 💬 **拉机器人进群**，和大家一起玩
- 🎨 **自定义设置**，让机器人更像你

记住：**机器人是活的**，它会慢慢学习，变得越来越懂你！

---

**遇到问题？** 加群问大家：[麦麦脑电图](https://qm.qq.com/q/RzmCiRtHEW)

**祝你玩得开心！** 🌟