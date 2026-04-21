---
title: 模型配置
---

# 设置AI大脑

`model_config.toml` 是给麦麦配置"AI大脑"的文件，决定了麦麦的不同组件使用什么LLM

我们推荐根据不同任务的特性来分配不同的模型

麦麦的配置必须一个LLM模型（或VLM），一个VLM模型和嵌入模型

## 配置文件结构

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

## API 提供商 [[api_providers]]

API 提供商代表提供LLM服务的对象

### 基础配置（必填）

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `name` | 服务商名字 | 自己取个名字，比如"deepseek"、"openai" |
| `base_url` | API地址 | 服务商提供的url |
| `api_key` | 密钥 | 注册后获得的密钥 |

### 常见服务商配置示例

::: code-group

```toml [DeepSeek]
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [OpenAI]
[[api_providers]]
name = "openai"
base_url = "https://api.openai.com/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [阿里百炼]
[[api_providers]]
name = "aliyun"
base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [字节火山]
[[api_providers]]
name = "volcengine"
base_url = "https://ark.cn-beijing.volces.com/api/v3"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

:::

### 高级配置（可选）

| 🏷️ 配置项 | 💡 作用 | 📊 推荐值 |
|-----------|--------|----------|
| `timeout` | 超时时间 | `10` 秒 |
| `max_retry` | 失败重试次数 | `2` 次 |
| `retry_interval` | 重试间隔 | `10` 秒 |

## 模型列表 [[models]]

模型就是具体的LLM，比如GPT-5.4、Claude-4.6-opus、DeepSeek V4等。

### 基础配置（必填）

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `name` | 模型名字 | 自己取个名字，比如"gpt-5"、"deepseek-v4" |
| `model_identifier` | 模型ID | 服务商提供的具体模型名称 |
| `api_provider` | 用哪个服务商 | 填上面api_providers（提供商）里的name |
| `visual` | 是否启用视觉 | 只有多模态模型可以打开此选项 |

### 模型配置示例

```toml
[[models]]
name = "deepseek-chat"                    # 我叫它"deepseek-chat"
model_identifier = "deepseek-chat"        # 服务商也这么叫
api_provider = "deepseek"                 # 用deepseek这个服务商
visual = false                            # 不能看图

[[models]]
name = "qwen3.5-vl"                          # 视觉模型，能看图
model_identifier = "qwen3.5-flash"
api_provider = "aliyun"
visual = true                             # ✅ 能看图
```


## 任务配置 [model_task_config]

你需要根据模型的特点分给各种任务，实现最好的表现和最优的效率

### 任务类型说明

| 🏷️ 任务 | 💡 干什么 | 🎯 推荐模型 | 示例模型 |
|----------|----------|------------|------------|
| `utils` | 工具类：表情包、学习分析 | 便宜实用的模型 | dsv4/qwen3.5-35A3B/gemini3.1-flash/gptmini |
| `planner` | 规划器：决定行动逻辑，搜集信息，何时回复等 | 实用的模型（需要支持tool调用 | dsv4/qwen3.5-35A3B/gemini3.1|
| `replyer` | 回复器：生成实际回复 | 质量好的模型 | dsv4(思考)/claude-4.6-opus/gemini3.1 |
| `vlm` | 看图说话：理解图片 | 视觉模型 | qwen3.5-35A3B/gemini3.1-flash |
| `embedding` | 生成向量：用于记忆搜索 | 嵌入模型 | qwen3-embbeding |

### 任务配置示例

```toml
# 工具类任务：用便宜实用的
[model_task_config.utils]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3

# 规划器：用实用的
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
model_list = ["qwen3.5-flash"]
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

