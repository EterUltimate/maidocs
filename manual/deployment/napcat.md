---
title: NapCat 适配器
---

# 📱 NapCat QQ 连接器

## 🤔 NapCat 是什么？

NapCat 就是让 MaiBot 能收发 QQ 消息的"桥梁"。你可以把它想象成一个特殊的 QQ 客户端，让机器人能够：

- ✅ 在 QQ 群里聊天
- ✅ 接收私聊消息  
- ✅ 发送表情、图片

## 🚀 部署方式（选一种就行）

| 方式 | 推荐度 | 说明 |
|------|--------|------|
| **Docker** | ⭐⭐⭐ | 最简单，推荐新手 |
| 本地安装 | ⭐⭐ | 高级用户用 |

::: tip 💡 新手建议
直接用 Docker！MaiBot 的 `docker-compose.yml` 已经包含了 NapCat，一键启动就行。
:::

## 🐳 Docker 部署（最简单）

如果你用 Docker 部署 MaiBot，NapCat 已经包含在内了！

启动后，NapCat 会在 `6099` 端口工作，就像这样：

```yaml
napcat:
  image: mlikiowa/napcat-docker:latest
  ports:
    - "6099:6099"  # QQ 连接端口
  volumes:
    - ./docker-config/napcat:/app/napcat/config
    - ./data/qq:/app/.config/QQ  # QQ 登录信息保存这里
```

## ⚙️ 配置 NapCat

### WebSocket 设置（照着填就行）

MaiBot 通过 WebSocket 连接 NapCat，配置很简单：

- **连接方式**：反向 WebSocket
- **地址**：`ws://localhost:6099`（本地）或 `ws://maim-bot-napcat:6099`（Docker）

配置文件位置：
- Docker：`./docker-config/napcat/`
- 本地：NapCat 安装目录的 config 文件夹

## 📱 QQ 登录（重要！）

第一次启动 NapCat 后，需要登录 QQ 账号：

### 方法 1：扫码登录（推荐）

```bash
# 看日志找二维码
docker compose logs -f napcat
```

### 方法 2：密码登录

在配置文件里填写账号密码（不推荐，容易封号）

::: warning ⚠️ 重要提醒
- **用小号！用小号！用小号！**（说三遍）
- 机器人账号有被封的风险
- 登录信息会保存在 `./data/qq/`，下次不用重新登录
:::

## 🔗 连接 MaiBot

### 1. 设置机器人账号

在 `bot_config.toml` 里填写：

```toml
[bot]
platform = "qq"           # 平台类型
qq_account = 123456789    # 机器人 QQ 号
nickname = "麦麦"          # 机器人昵称
```

### 2. 启动顺序

1. **先启动 NapCat** → 等 QQ 登录成功
2. **再启动 MaiBot** → 开始聊天

Docker 用户直接：
```bash
docker compose up -d
```

Docker 会自动按正确顺序启动。

## 🛠️ 常见问题

### 连不上 NapCat？

检查这几件事：
- NapCat 启动了吗？`docker compose logs napcat`
- 端口 6099 能访问吗？浏览器打开 `http://localhost:6099`
- QQ 号填对了吗？

### QQ 登录失败？

- **扫码超时**：重启 NapCat 获取新二维码
- **设备锁**：按提示验证
- **频繁掉线**：换网络，或等一会儿再试

### 消息发不出去？

1. NapCat 和 MaiBot 在同一网络吗？
2. WebSocket 端口设置对了吗？
3. 看日志：`docker compose logs core`

### NapCat 版本太旧？

```bash
# 更新到最新版
docker compose pull napcat
docker compose up -d napcat
```

## 🎯 成功标志

如果一切正常，你会看到：
- NapCat 日志显示 QQ 登录成功
- MaiBot 日志显示连接到 NapCat
- 机器人能在 QQ 群里说话了！

恭喜！你的 MaiBot 已经可以和大家聊天了 🎉
