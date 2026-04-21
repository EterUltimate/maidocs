---
title: 💬 QQ 机器人连接指南
---

# 💬 QQ 机器人连接指南

想让 MaiBot 在 QQ 群里陪你聊天？用 NapCat 就能轻松搞定！

## 什么是 NapCat？🤔

NapCat 就像一个"翻译官"，帮 MaiBot 和 QQ 聊天：
- ✅ 不需要装完整的 QQ 客户端
- ✅ 稳定、快速、省资源  
- ✅ 支持群聊和私聊
- ✅ 自动连接，不用你操心

简单来说：NapCat 让 MaiBot 能够"听懂"QQ 消息，也能"说话"给 QQ 听！

## 怎么工作的？🔄

就像打电话一样简单：
1. **NapCat** 登录你的 QQ 号（就像你用手机登录 QQ）
2. **MaiBot** 启动后等着连接（就像接电话）
3. **NapCat** 自动找到 MaiBot 并连上（就像拨号就能通话）
4. **消息传递** QQ 消息 → NapCat → MaiBot → 回复 → NapCat → QQ

## 配置 MaiBot 🛠️

### 第一步：告诉 MaiBot 你要用 QQ

在 `config/bot_config.toml` 里添加：

```toml
[bot]
platform = "qq"           # 用 QQ 平台
qq_account = 123456789    # 你的机器人 QQ 号
nickname = "麦麦"          # 机器人昵称
```

就这么简单！🎉

### 第二步：设置连接参数

还是在 `config/bot_config.toml` 里：

```toml
[maim_message]
ws_server_host = "127.0.0.1"   # 服务器地址（本地就用这个）
ws_server_port = 8080           # 端口号（默认 8080）
auth_token = []                 # 认证令牌，空着就行
```

| 配置项 | 什么意思 | 怎么填 |
|--------|----------|--------|
| `ws_server_host` | 服务器地址 | 本地用 `127.0.0.1`，服务器用实际 IP |
| `ws_server_port` | 端口号 | 默认 `8080`，改了就记住这个数字 |
| `auth_token` | 密码验证 | 空着就行，不用管 |

## 配置 NapCat 🐱

### 安装 NapCat

两种方式，任选其一：

**方式一：Docker（推荐）**

如果你使用项目自带的 `docker-compose.yml`，NapCat 已经作为 `napcat` 服务包含在内，直接和 MaiBot 一起启动即可：

```bash
docker compose up -d
```

对应服务配置如下：

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

启动后，NapCat WebUI 默认暴露在 `6099` 端口：

- 本机访问：`http://localhost:6099`
- 配置目录：`./docker-config/napcat/`
- QQ 登录数据：`./data/qq/`
- 查看日志：`docker compose logs -f napcat`

**方式二：本地安装**
从 [NapCatQQ Release](https://github.com/NapNeko/NapCatQQ/releases) 下载对应版本

### 设置连接

1. 打开 NapCat 的网页界面
2. 找到 "反向 WebSocket" 设置
3. 填上 MaiBot 地址：`ws://127.0.0.1:8080/ws`

如果 NapCat 和 MaiBot 都在 Docker Compose 里运行，请确认 MaiBot 的 `maim_message.ws_server_host` 监听地址允许容器网络访问；不确定时优先使用项目自带的 Docker 配置。

配置示例：
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

### 登录 QQ

启动 NapCat 后需要登录：

1. **扫码登录**：看日志里的二维码，用手机 QQ 扫一下
2. **密码登录**：在配置里填账号密码

⚠️ **重要提醒**：
- 建议用小号，降低被封风险
- 登录信息会保存，重启后不用重新登录
- 遵守 QQ 规则，别发垃圾消息

## 连接步骤 📋

### 推荐启动顺序：

1. **启动 NapCat** → 等 QQ 登录成功
2. **启动 MaiBot** → 等 WebSocket 服务启动  
3. **自动连接** → NapCat 会自动连上 MaiBot

```bash
# Docker 一键启动（推荐）
docker compose up -d

# 手动启动
# 终端 1：启动 NapCat
# 终端 2：uv run python bot.py
```

### 验证连接 ✅

怎么知道连上了？看这几个地方：

1. **MaiBot 日志**：看到 "WebSocket 服务启动成功"
2. **NapCat 日志**：看到 "反向 WebSocket 连接成功"
3. **发消息测试**：在 QQ 群里 @机器人，看有没有回复

## 常见问题 🤔

### 连不上怎么办？

**检查这几点**：
- 地址和端口填对了吗？
- 防火墙拦住了吗？
- NapCat 和 MaiBot 在同一台机器吗？
- 日志里有什么报错信息？

### 收不到消息？

**可能原因**：
- QQ 号填错了？要和 NapCat 登录的一致
- NapCat 本身收到消息了吗？看 NapCat 日志
- 网络连接正常吗？

### 发不出消息？

**排查方法**：
- 机器人有发言权限吗？（群聊需要权限）
- 看 MaiBot 日志有什么报错
- 是不是多个程序同时用一个 QQ 号？

### 其他问题

- **版本兼容性**：用最新版 NapCat 一般没问题
- **网络问题**：检查防火墙和网络设置
- **权限问题**：确保机器人有必要的权限

### 怎么更新 NapCat？

Docker 部署可以这样更新：

```bash
docker compose pull napcat
docker compose up -d napcat
```

## 安全提醒 🔒

- 别把 WebSocket 端口暴露到公网
- 用 `127.0.0.1` 比较安全
- 定期更新 NapCat 版本
- 注意 QQ 的使用规则

## 获取帮助 💬

遇到问题可以：
- 查看 NapCat 和 MaiBot 的日志
- 加入 MaiBot 交流群问问
- 在 GitHub 提交问题
- 搜索相关教程

祝你连接顺利，让 MaiBot 在 QQ 里陪你聊天！🎉
