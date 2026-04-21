---
title: NapCat Adapter
---

# NapCat Adapter

## What is NapCat

[NapCat](https://github.com/NapNeko/NapCatQQ) is a modern Bot protocol implementation based on NTQQ. MaiBot connects to the QQ platform through NapCat to receive and send messages.

NapCat runs QQ client in headless mode based on QQ NT architecture, and communicates with MaiBot through WebSocket or HTTP interface.

## Deployment Methods

NapCat supports multiple deployment methods:

| Method | Description |
|------|------|
| Docker | Deploy through `mlikiowa/napcat-docker` image (recommended) |
| Local Installation | Run NapCat directly on the host |

::: tip Recommendation
Docker deployment is the simplest, MaiBot's `docker-compose.yml` already includes NapCat service.
:::

### Docker Deployment (Recommended)

In `docker-compose.yml`, NapCat service is defined as follows:

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
  networks:
    - maim_bot
```

After startup, NapCat will provide WebSocket service on port `6099`.

## NapCat Configuration

### WebSocket Mode

MaiBot connects to NapCat in **WebSocket client** mode. You need to configure reverse WebSocket in NapCat.

NapCat configuration file path (Docker deployment): `./docker-config/napcat/`

Key configuration items:

- **Connection Method**: Reverse WebSocket (MaiBot actively connects to NapCat)
- **Listen Address**: `ws://maim-bot-napcat:6099` (within Docker network) or `ws://localhost:6099` (local deployment)

### QQ Login

After first startup of NapCat, you need to log in to QQ account. There are two ways:

1. **QR Code Login**: Check NapCat container logs to get QR code
2. **Account Password Login**: Set in NapCat configuration

```bash
# View NapCat logs to get login QR code
docker compose logs -f napcat
```

::: warning
- QQ accounts have risk of being banned, it is recommended to use a secondary account
- Login data is persisted in `./data/qq` directory, no need to re-login after restart
:::

## Connect to MaiBot

### 1. Configure bot_config.toml

Set platform information and QQ account in `bot_config.toml`:

```toml
[bot]
platform = "qq"           # Platform identifier
qq_account = 123456789    # Your QQ bot account
nickname = "麦麦"          # Bot nickname
```

### 2. Configure Message Adapter

MaiBot handles platform communication through the `maim-message` library. Adapter configuration affects how MaiBot establishes connection with NapCat.

In Docker environment, adapter configuration file is located at `./docker-config/adapters/config.toml`, you need to configure WebSocket connection address:

```toml
# Adapter connection configuration example
# Specific format depends on maim-message version
```

### 3. Startup Sequence

Recommended startup sequence:

1. Start NapCat → Wait for QQ login success
2. Start MaiBot core service

```bash
# Docker method (docker compose will automatically handle dependencies)
docker compose up -d

# Manual method
# Terminal 1: Start NapCat
# Terminal 2: uv run python bot.py
```

## Common Issues

### Connection Failed

- Check if NapCat started normally: `docker compose logs napcat`
- Check if WebSocket port is reachable: `curl http://localhost:6099`
- Check if `platform` and `qq_account` in MaiBot configuration are correct

### QQ Login Issues

- **QR Code Timeout**: Restart NapCat container to get new QR code
- **Device Lock Verification**: Complete verification as prompted
- **Frequent Disconnection**: May be QQ risk control, try changing network environment

### Messages Cannot Be Sent/Received

1. Confirm NapCat and MaiBot are in the same network
2. Check if NapCat's WebSocket port configuration is consistent with MaiBot adapter configuration
3. View MaiBot logs: `docker compose logs core`

### NapCat Version Compatibility

NapCat is continuously being updated. If you encounter problems, please upgrade to the latest version first:

```bash
docker compose pull napcat
docker compose up -d napcat
```