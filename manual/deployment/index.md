---
title: 部署概览
---

# 🚀 MaiBot 部署概览

**MaiBot 就是一个智能聊天机器人**，可以陪你聊天、讲笑话、回答问题。你只需要准备两样东西就能让它跑起来：

📱 **一个 QQ 小号**（用来登录机器人）
🔑 **一个 AI 模型的 API 密钥**（就像一把钥匙，让机器人能思考说话）

## 📦 部署方式选择

MaiBot 提供 3 种安装方式，任选其一即可：

| 方式 | 适合人群 | 难度 |
|------|---------|------|
| [源码安装](./installation.md) | 想自己折腾、控制细节的用户 | ⭐⭐ |
| [Docker 部署](./docker.md) | 想一键部署、服务器用户 | ⭐ |
| [NapCat 适配器](./napcat.md) | 需要连接 QQ 的用户 | ⭐⭐ |

::: tip 💡 新手推荐
第一次用？建议按这个顺序：

1. **先安装 MaiBot** → [安装指南](./installation.md)
2. **再连接 QQ** → [NapCat 适配器](./napcat.md)
3. **最后配置 AI 模型** → [模型配置](../configuration/model-config.md)
:::

## ⚡ 3 分钟快速开始

如果你已经准备好了 Python 环境和 AI API，可以直接开干：

```bash
# 1. 下载 MaiBot
git clone https://github.com/MaiM-with-u/MaiBot.git
cd MaiBot

# 2. 安装依赖（就像给机器人装零件）
uv sync

# 3. 启动！
uv run python bot.py
```

第一次启动会要求你同意用户协议，输入"同意"就行。就这么简单！
