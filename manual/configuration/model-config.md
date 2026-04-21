---
title: 模型配置
---

# 🧠 设置AI大脑

`model_config.toml` 是给麦麦配置"AI大脑"的文件，就像给她选择用哪个品牌的手机一样 - 不同品牌功能差不多，但各有特色。

## 📋 配置文件结构

```toml
# 配置文件版本
[inner]
version = "1.14.0"

# API 提供商列表（AI服务商）
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-xxxxxxxxxxxxxxxxxxxxxxxx"

# 模型列表（具体用哪个AI）
[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

# 任务分配（不同工作用不同AI）
[model_task_config.replyer]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
```

## 🏪 API 提供商 [[api_providers]]

API 提供商就像不同的手机品牌：苹果、华为、小米，都能打电话，但各有特点。

### 基础配置（必填）

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `name` | 服务商名字 | 自己取个名字，比如"deepseek"、"openai" |
| `base_url` | API地址 | 服务商提供的网址 |
| `api_key` | 密钥 | 注册后获得的密钥，像密码一样 |

### 常见服务商配置示例

::: code-group

```toml [DeepSeek - 推荐👍]
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [OpenAI - 原版]
[[api_providers]]
name = "openai"
base_url = "https://api.openai.com/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [SiliconFlow - 国内友好]
[[api_providers]]
name = "siliconflow"
base_url = "https://api.siliconflow.cn/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [本地模型 - 高级玩家]
[[api_providers]]
name = "local"
base_url = "http://localhost:8080/v1"
api_key = ""                    # 本地可以留空
client_type = "openai"
auth_type = "none"              # 本地不需要认证
```

:::

### 高级配置（可选）

| 🏷️ 配置项 | 💡 作用 | 📊 推荐值 |
|-----------|--------|----------|
| `timeout` | 超时时间 | `10` 秒 |
| `max_retry` | 失败重试次数 | `2` 次 |
| `retry_interval` | 重试间隔 | `10` 秒 |

## 🤖 模型列表 [[models]]

模型就是具体的AI，比如GPT-4、Claude、DeepSeek等。

### 基础配置（必填）

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `name` | 模型名字 | 自己取个名字，比如"gpt-4"、"deepseek-chat" |
| `model_identifier` | 模型ID | 服务商提供的具体模型名称 |
| `api_provider` | 用哪个服务商 | 填上面api_providers里的name |

### 模型配置示例

```toml
[[models]]
name = "deepseek-chat"                    # 我叫它"deepseek-chat"
model_identifier = "deepseek-chat"        # 服务商也这么叫
api_provider = "deepseek"                 # 用deepseek这个服务商
visual = false                            # 不能看图

[[models]]
name = "qwen-vl"                          # 视觉模型，能看图
model_identifier = "qwen-vl-plus"
api_provider = "aliyun"
visual = true                             # ✅ 能看图
```

::: tip 💡 小贴士
`visual = true` 表示这个AI能看懂图片，需要看图功能就选这种。
:::

## 🎯 任务配置 [model_task_config]

不同工作用不同AI，就像不同任务用不同工具：写作用好笔，草稿用普通笔。

### 任务类型说明

| 🏷️ 任务 | 💡 干什么 | 🎯 推荐模型 |
|----------|----------|------------|
| `utils` | 工具类：表情包、取名、关系分析 | 便宜实用的模型 |
| `planner` | 规划器：决定要不要回复 | 便宜实用的模型 |
| `replyer` | 回复器：生成实际回复 | 质量好的模型 |
| `vlm` | 看图说话：理解图片 | 视觉模型 |
| `embedding` | 生成向量：用于记忆搜索 | 专用嵌入模型 |

### 任务配置示例

```toml
# 工具类任务：用便宜实用的
[model_task_config.utils]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3

# 规划器：用便宜实用的
[model_task_config.planner]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3

# 回复器：用好一点的模型
[model_task_config.replyer]
model_list = ["deepseek-chat"]  # 或者gpt-4等更好的
max_tokens = 1024
temperature = 0.7

# 看图：用视觉模型
[model_task_config.vlm]
model_list = ["qwen-vl"]
max_tokens = 1024
temperature = 0.3
```

### 参数说明

| 🏷️ 参数 | 💡 是什么 | 📊 推荐值 |
|----------|----------|----------|
| `max_tokens` | 最多输出多少字 | `1024` |
| `temperature` | 创造性（0-2） | `0.3` 保守，`0.7` 有创意 |
| `model_list` | 用哪些模型 | 可以写多个，自动切换 |

## 🎯 推荐配置（新手专用）

### 方案1：单模型配置（最简单）

```toml
# 只用DeepSeek，所有任务都用它
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-你的密钥"

[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

[model_task_config.utils]
model_list = ["deepseek-chat"]

[model_task_config.planner]
model_list = ["deepseek-chat"]

[model_task_config.replyer]
model_list = ["deepseek-chat"]
```

### 方案2：分工配置（性价比）

```toml
# 工具类用便宜的，回复用好的
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-你的密钥"

[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

[model_task_config.utils]
model_list = ["deepseek-chat"]  # 工具类用便宜的

[model_task_config.planner]
model_list = ["deepseek-chat"]  # 规划也用便宜的

[model_task_config.replyer]
model_list = ["deepseek-chat"]  # 回复可以用好一点的（等你有更好的模型再换）
```

## 🚨 常见问题

### Q: 哪个AI最好用？
A: 新手推荐DeepSeek，便宜好用。有钱可以试试GPT-4。

### Q: 需要买API吗？
A: 是的，大部分AI服务商都要充值。DeepSeek比较便宜。

### Q: 配置错了怎么办？
A: 改对了再重启就行，配置文件会自动备份。

### Q: 可以同时用多个AI吗？
A: 可以！model_list可以写多个，会自动切换。

## 🚀 下一步看什么？

- 🆕 **新手推荐**：配置好了就启动试试！
- 🎭 **想调性格**：看 [人格配置](./personality-config.md)
- 💬 **想调聊天**：看 [聊天配置](./chat-config.md)
- 🧠 **想开记忆**：看 [记忆配置](./memory-config.md)