---
title: 安装指南
---

# 📦 MaiBot 安装指南

本文将手把手教你安装和启动 MaiBot，就像拼装一个智能玩具！

## 📥 下载 MaiBot

从 [GitHub Release](https://github.com/MaiM-with-u/MaiBot/releases/) 下载最新版本，或者直接克隆仓库：

::: code-group

```bash [稳定版 (推荐)]
git clone https://github.com/MaiM-with-u/MaiBot.git
cd MaiBot
```

```bash [开发版 (尝鲜)]
git clone -b dev https://github.com/MaiM-with-u/MaiBot.git
cd MaiBot
```

:::

::: warning ⚠️ 注意
`dev` 分支有新功能但可能不稳定。第一次用建议选 `main` 分支。
:::

## 🔧 安装依赖

MaiBot 用 [uv](https://github.com/astral-sh/uv) 管理依赖（就像 pip，但更快更好用）。

### 安装 uv（就像给电脑装个新工具）

::: code-group

```bash [Windows]
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

```bash [macOS / Linux]
curl -LsSf https://astral.sh/uv/install.sh | sh
```

:::

### 安装项目依赖（给机器人装零件）

::: code-group

```bash [推荐：uv]
uv sync
```

```bash [备用：pip]
pip install -r requirements.txt
```

:::

`uv sync` 会自动装好所有需要的组件，包括数据库、Web界面等。

## ⚙️ 配置机器人

MaiBot 的配置文件在 `config/` 目录下，就像机器人的"大脑设置"：

| 文件 | 作用 | 看这里 |
|------|------|------|
| `bot_config.toml` | 机器人基本信息 | [配置概览](../configuration/index.md) |
| `model_config.toml` | AI模型设置 | [模型配置](../configuration/model-config.md) |

第一次启动时，MaiBot 会自动生成默认配置。你需要改两个地方：

1. **设置 QQ 账号**：在 `bot_config.toml` 中填写 `qq_account`（你的机器人 QQ 号）
2. **设置 AI 模型**：在 `model_config.toml` 中填写 `api_key`（你的 AI 服务密钥）

## 🚀 启动机器人

```bash
uv run python bot.py
```

## ✅ 用户协议确认

第一次启动会要求你同意用户协议，很简单：

**在终端输入"同意"就行！** 不需要记什么哈希值。

## 🔍 常见问题（新手别怕）

### Python 版本不对？

MaiBot 需要 Python 3.10 以上。检查版本：

```bash
python --version
```

::: tip 💡 不确定选哪个版本？
选最新的 Python 3.13 准没错！
:::

### 依赖装不上？

```bash
# 清除缓存重新装
uv sync --reinstall
```

### 启动卡住了？

- 检查配置文件填对了没
- 看看是不是网络问题
- 重启试试：Ctrl+C 停止，再运行一次

### 想重启机器人？

直接 Ctrl+C 停止，再运行 `uv run python bot.py` 就行。机器人会自动恢复状态。
