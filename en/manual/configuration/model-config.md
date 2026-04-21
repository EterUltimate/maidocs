---
title: Model Configuration
---

# Set Up the AI Brain

`model_config.toml` is the file used to configure MaiMai's "AI brain". It decides which LLM each MaiMai component uses.

We recommend assigning different models based on the characteristics of different tasks.

MaiMai's configuration must include one LLM model (or VLM), one VLM model, and one embedding model.

## Configuration File Structure

```toml
# Configuration file version
[inner]
version = "1.14.0"

# API provider list (AI service providers)
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-xxxxxxxxxxxxxxxxxxxxxxxx"

# Model list (which specific AI to use)
[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

# Task assignment (different work uses different AI)
[model_task_config.replyer]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
```

## API Providers [[api_providers]]

API providers represent the services that provide LLM capabilities.

### Basic Configuration (Required)

| 🏷️ Item | 💡 What It Is | 📝 How to Fill |
|-----------|----------|----------|
| `name` | Provider name | Choose a name yourself, such as "deepseek" or "openai" |
| `base_url` | API address | The URL provided by the service provider |
| `api_key` | Key | The key you get after registration |

### Common Provider Configuration Examples

::: code-group

```toml [DeepSeek]
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

```toml [OpenAI]
[[api_providers]]
name = "openai"
base_url = "https://api.openai.com/v1"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

```toml [Alibaba Bailian]
[[api_providers]]
name = "aliyun"
base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

```toml [ByteDance Volcengine]
[[api_providers]]
name = "volcengine"
base_url = "https://ark.cn-beijing.volces.com/api/v3"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

:::

### Advanced Configuration (Optional)

| 🏷️ Item | 💡 Purpose | 📊 Recommended Value |
|-----------|--------|----------|
| `timeout` | Timeout | `10` seconds |
| `max_retry` | Retry count after failure | `2` times |
| `retry_interval` | Retry interval | `10` seconds |

## Model List [[models]]

Models are specific LLMs, such as GPT-5.4, Claude-4.6-opus, DeepSeek V4, and so on.

### Basic Configuration (Required)

| 🏷️ Item | 💡 What It Is | 📝 How to Fill |
|-----------|----------|----------|
| `name` | Model name | Choose a name yourself, such as "gpt-5" or "deepseek-v4" |
| `model_identifier` | Model ID | The specific model name provided by the service provider |
| `api_provider` | Which provider to use | Fill in the `name` from `api_providers` above |
| `visual` | Whether to enable vision | Only multimodal models can enable this option |

### Model Configuration Example

```toml
[[models]]
name = "deepseek-chat"                    # I call it "deepseek-chat"
model_identifier = "deepseek-chat"        # The provider also calls it this
api_provider = "deepseek"                 # Use the deepseek provider
visual = false                            # Cannot see images

[[models]]
name = "qwen3.5-vl"                       # Vision model, can see images
model_identifier = "qwen3.5-flash"
api_provider = "aliyun"
visual = true                             # ✅ Can see images
```

## Task Configuration [model_task_config]

You need to assign models to different tasks based on model characteristics to achieve the best performance and efficiency.

### Task Type Description

| 🏷️ Task | 💡 What It Does | 🎯 Recommended Model | Example Model |
|----------|----------|------------|------------|
| `utils` | Tool tasks: emoji, learning analysis | Cheap and practical model | dsv4/qwen3.5-35A3B/gemini3.1-flash/gptmini |
| `planner` | Planner: decides action logic, collects information, decides when to reply, etc. | Practical model (needs tool calling support | dsv4/qwen3.5-35A3B/gemini3.1 |
| `replyer` | Replyer: generates the actual reply | High-quality model | dsv4(thinking)/claude-4.6-opus/gemini3.1 |
| `vlm` | Image understanding: talks about pictures | Vision model | qwen3.5-35A3B/gemini3.1-flash |
| `embedding` | Generate vectors: used for memory search | Embedding model | qwen3-embbeding |

### Task Configuration Example

```toml
# Tool tasks: use cheap and practical models
[model_task_config.utils]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3

# Planner: use a practical model
[model_task_config.planner]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3

# Replyer: use a better model
[model_task_config.replyer]
model_list = ["deepseek-chat"]  # Or GPT-4 and other better models
max_tokens = 1024
temperature = 0.7

# Images: use a vision model
[model_task_config.vlm]
model_list = ["qwen3.5-flash"]
max_tokens = 1024
temperature = 0.3
```

### Parameter Description

| 🏷️ Parameter | 💡 What It Is | 📊 Recommended Value |
|----------|----------|----------|
| `max_tokens` | Maximum output length | `1024` |
| `temperature` | Creativity (0-2) | `0.3` conservative, `0.7` creative |
| `model_list` | Which models to use | Multiple models can be written, automatically switched |

## 🎯 Recommended Configuration (For Beginners)

### Option 1: Single Model Configuration (Simplest)

```toml
# Only use DeepSeek, all tasks use it
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-your-key"

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

### Option 2: Task Split Configuration (Cost-Effective)

```toml
# Use cheaper models for tools and better models for replies
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-your-key"

[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

[model_task_config.utils]
model_list = ["deepseek-chat"]  # Use cheap model for tool tasks

[model_task_config.planner]
model_list = ["deepseek-chat"]  # Also use cheap model for planning

[model_task_config.replyer]
model_list = ["deepseek-chat"]  # Replies can use a better model later when you have one
```
