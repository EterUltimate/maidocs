---
title: Model Configuration
---

# Model Configuration

`model_config.toml` controls how MaiBot connects to and uses LLM APIs. It contains three core parts: API providers, model list, and task assignment.

## Configuration File Structure

```toml
# Configuration file version
[inner]
version = "1.14.0"

# API provider list
[[api_providers]]
name = "..."
base_url = "..."
api_key = "..."

# Model list
[[models]]
name = "..."
model_identifier = "..."
api_provider = "..."

# Task and model assignment
[model_task_config.utils]
model_list = ["..."]
# ...
```

## API Providers [[api_providers]]

API providers define LLM API connection information. You can configure multiple providers, each model uses one of them.

### Basic Fields

| Field | Type | Required | Description |
|------|------|------|------|
| `name` | `str` | Yes | Provider name (custom, used when models reference) |
| `base_url` | `str` | Yes | API endpoint Base URL |
| `api_key` | `str` | Yes | API key |
| `client_type` | `str` | No | Client type, optional `"openai"` or `"google"`, default `"openai"` |

### Authentication Configuration

| Field | Type | Default | Description |
|------|------|--------|------|
| `auth_type` | `str` | `"bearer"` | Authentication method: `bearer`, `header`, `query`, `none` |
| `auth_header_name` | `str` | `"Authorization"` | Request header name when `auth_type=header` |
| `auth_header_prefix` | `str` | `"Bearer"` | Request header prefix when `auth_type=header` |
| `auth_query_name` | `str` | `"api_key"` | Query parameter name when `auth_type=query` |

::: tip Authentication Methods
- `bearer` (default): Standard Bearer Token authentication, suitable for most OpenAI compatible APIs
- `header`: Custom request header authentication
- `query`: URL query parameter authentication
- `none`: No authentication required (`api_key` can be left empty)
:::

### Request Configuration

| Field | Type | Default | Description |
|------|------|--------|------|
| `timeout` | `int` | `10` | Single API call timeout (seconds) |
| `max_retry` | `int` | `2` | Maximum retry attempts |
| `retry_interval` | `int` | `10` | Retry interval (seconds) |
| `default_headers` | `dict` | `{}` | Default additional HTTP request headers |
| `default_query` | `dict` | `{}` | Default additional query parameters |
| `organization` | `str` | `None` | OpenAI official interface organization |
| `project` | `str` | `None` | OpenAI official interface project |

### Parsing Strategy

| Field | Type | Default | Description |
|------|------|--------|------|
| `reasoning_parse_mode` | `str` | `"auto"` | Reasoning content parsing: `auto`, `native`, `think_tag`, `none` |
| `tool_argument_parse_mode` | `str` | `"auto"` | Tool parameter parsing: `auto`, `strict`, `repair`, `double_decode` |

### Configuration Examples

::: code-group

```toml [OpenAI Compatible Interface]
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-xxxxxxxxxxxxxxxxxxxxxxxx"
client_type = "openai"
auth_type = "bearer"
timeout = 10
max_retry = 2
retry_interval = 10
```

```toml [Google Gemini]
[[api_providers]]
name = "google"
base_url = ""
api_key = "AIzaxxxxxxxxxxxxxxxx"
client_type = "google"
auth_type = "bearer"
timeout = 10
max_retry = 2
```

```toml [Local Interface Without Authentication]
[[api_providers]]
name = "local-llm"
base_url = "http://localhost:8080/v1"
api_key = ""
client_type = "openai"
auth_type = "none"
timeout = 30
```

:::

## Model List [[models]]

Each model entry defines an available LLM model.

### Basic Fields

| Field | Type | Required | Description |
|------|------|------|------|
| `name` | `str` | Yes | Model name (custom, used when task configuration) |
| `model_identifier` | `str` | Yes | Model identifier provided by API service provider |
| `api_provider` | `str` | Yes | Corresponding `name` in `api_providers` |
| `visual` | `bool` | No | Whether supports multimodal (visual input), default `false` |

### Advanced Fields

| Field | Type | Default | Description |
|------|------|--------|------|
| `price_in` | `float` | `0.0` | Input price (yuan/M Token), used for statistics |
| `price_out` | `float` | `0.0` | Output price (yuan/M Token), used for statistics |
| `temperature` | `float` | `None` | Model-level temperature, overrides temperature in task configuration |
| `max_tokens` | `int` | `None` | Model-level max Token, overrides task configuration |
| `force_stream_mode` | `bool` | `false` | Force streaming output (for models that don't support non-streaming) |
| `extra_params` | `dict` | `{}` | Additional parameters during API calls |

### Configuration Example

```toml
[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"
price_in = 0.0
price_out = 0.0
visual = false

[[models]]
name = "qwen-vl"
model_identifier = "qwen-vl-plus"
api_provider = "aliyun"
visual = true   # Supports visual input
```

::: tip Visual Models
If the model supports image input, set `visual` to `true`. MaiBot will automatically decide whether to enable multimodal mode based on this flag and [Visual Configuration](./personality-config.md#visual-configuration-visual).
:::

## Task Configuration [model_task_config]

Task configuration defines which models MaiBot's various functional modules use. Each task can configure multiple models, and the selection strategy determines which one to use for each call.

### Task Types

| Task | Description |
|------|------|
| `utils` | Tool tasks: emoji module, naming module, relationship module, mood changes, etc. |
| `planner` | Planner: Analyze conversation context, decide whether to reply and reply strategy |
| `replyer` | Replyer: Generate actual reply content |
| `vlm` | Visual model: Handle image understanding tasks |
| `voice` | Voice recognition: Convert voice messages to text |
| `embedding` | Embedding model: Generate text vectors for memory retrieval |

### Configuration Fields for Each Task

| Field | Type | Default | Description |
|------|------|--------|------|
| `model_list` | `list[str]` | `[]` | List of model names used (corresponding to `name` in `models`) |
| `max_tokens` | `int` | `1024` | Maximum output Token count |
| `temperature` | `float` | `0.3` | Model temperature (0-2) |
| `slow_threshold` | `float` | `15.0` | Slow request threshold (seconds), outputs warning when exceeded |
| `selection_strategy` | `str` | `"balance"` | Model selection strategy: `balance` (load balancing) or `random` (random selection) |

### Configuration Example

```toml
[model_task_config.utils]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
selection_strategy = "balance"

[model_task_config.planner]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
selection_strategy = "balance"

[model_task_config.replyer]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
selection_strategy = "balance"

[model_task_config.vlm]
model_list = ["qwen-vl"]
max_tokens = 1024
temperature = 0.3
selection_strategy = "balance"

[model_task_config.embedding]
model_list = ["embedding-model"]
max_tokens = 512
temperature = 0.0
selection_strategy = "balance"
```

## Model Selection Strategy

When `model_list` contains multiple models, MaiBot calls models through `LLMOrchestrator` according to the selection strategy:

### balance (Load Balancing)

Default strategy. Round-robin distributes requests to each model in `model_list`, achieving load balancing. If a model call fails, it automatically falls back to other models.

### random (Random Selection)

Each request randomly selects a model from `model_list`. Suitable for scenarios where multiple models with equal capabilities are used as backup pools.

## Fault Tolerance and Retry

MaiBot's LLM calls have the following fault tolerance mechanisms:

1. **API Provider Level Retry**: After a single API call fails, retry according to `max_retry` and `retry_interval` configuration
2. **Model Level Fallback**: When all preferred models in `model_list` fail after retry, automatically try the next model in the list
3. **Timeout Protection**: Each API call is limited by `timeout`, preventing request hanging

## Common Configuration Scenarios

### Single Model Configuration

The simplest configuration, using only one API provider and one model:

```toml
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-xxxx"

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

### Multi-Model Shunting

Assign different tasks to models with different capabilities to reduce costs:

```toml
# Use cheap models for tools/planning, better models for replies
[model_task_config.utils]
model_list = ["cheap-model"]

[model_task_config.planner]
model_list = ["cheap-model"]

[model_task_config.replyer]
model_list = ["premium-model"]
```

### Load Balancing

Configure multiple models for the same task, automatic load balancing:

```toml
[model_task_config.replyer]
model_list = ["model-a", "model-b"]
selection_strategy = "balance"
```

### Visual Model

Configure specialized visual models for image understanding tasks:

```toml
[[models]]
name = "qwen-vl"
model_identifier = "qwen-vl-plus"
api_provider = "aliyun"
visual = true   # Mark as visual model

[model_task_config.vlm]
model_list = ["qwen-vl"]
```