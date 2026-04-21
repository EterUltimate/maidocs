---
title: TTS Adapter
---

# TTS Adapter

TTS (Text-to-Speech) adapter provides MaiBot with voice synthesis and recognition capabilities, allowing MaiMai to send and receive voice messages. MaiBot's voice functionality is divided into **ASR (Automatic Speech Recognition)** and **TTS (Text-to-Speech)** directions, implemented through different configurations and model services.

## Architecture Overview

MaiBot's voice processing includes two pathways:

```
Inbound Voice (Received Voice Messages)
    → ASR (Speech Recognition)
    → LLMServiceClient · voice task
    → Text → Enter Message Processing Pipeline

Outbound Voice (Reply Voice Messages)
    → TTS (Speech Synthesis)
    → LLMServiceClient · TTS task
    → Audio Data → Deliver to Platform via Platform IO
```

- **ASR (Speech Recognition)**: Converts received voice messages to text, controlled by `VoiceConfig.enable_asr`. When enabled, the `get_voice_text()` function in `utils_voice.py` uses the `voice` task type of `LLMServiceClient` to call the audio transcription interface, transcribing base64-encoded audio data to text
- **TTS (Speech Synthesis)**: Synthesizes text into voice for sending, implemented through the task distribution mechanism in `model_config.toml`, handing text to the configured TTS service for audio synthesis

## Supported TTS Backends

MaiBot supports multiple TTS backend services, all integrated through API calls. The essence of TTS backends is to declare an API provider and corresponding model in `model_config.toml`, then call through the task routing mechanism of `LLMServiceClient`.

### GPT-SoVITS

GPT-SoVITS is an open-source few-shot voice cloning and synthesis system, which MaiBot calls through its provided API interface.

**Features**:
- Supports few-shot voice cloning, only needs a small amount of reference audio to synthesize custom voice
- Completely open source, can be self-deployed
- Supports multi-language synthesis in Chinese, English, and Japanese
- Requires certain GPU resources for inference

**Deployment Method**:
1. Clone the [GPT-SoVITS](https://github.com/RVC-Boss/GPT-SoVITS) repository
2. Complete model training according to official documentation or use pre-trained models
3. Start API service (default port is usually `9880`)

**MaiBot Configuration Method**: Add GPT-SoVITS API address and parameters as an API provider in `model_config.toml`, then specify using this provider in the corresponding TTS task.

### Doubao TTS

Doubao TTS is a speech synthesis service provided by ByteDance through Volcano Engine, offering high-quality Chinese speech synthesis capabilities.

**Features**:
- High-quality Chinese speech synthesis with natural voice
- Multiple preset voices to choose from
- Pay-per-use, no need to build your own service
- Low latency, suitable for real-time interaction scenarios

**Configuration Method**: Add Volcano Engine's API key and endpoint as an API provider in `model_config.toml`. Need to enable speech synthesis service on Volcano Engine platform and obtain API Key.

### Qianwen Omni

Qianwen Omni is Alibaba Cloud's Tongyi Qianwen multimodal model, supporting text-to-speech synthesis capabilities.

**Features**:
- Alibaba Cloud Tongyi series model, excellent Chinese effect
- Supports multimodal interaction (text, voice, image)
- Calls through OpenAI-compatible API, simple integration
- Pay-per-use

**Configuration Method**: Add Qianwen Omni's API key and endpoint in `model_config.toml`, using OpenAI-compatible interface format.

## Voice Configuration in bot_config.toml

Voice-related configurations are located in the `[voice]` section of `bot_config.toml`:

```toml
[voice]
enable_asr = false   # Whether to enable speech recognition
```

### Configuration Item Description

| Configuration Item | Type | Default Value | Description |
|-------------------|------|---------------|-------------|
| `enable_asr` | bool | `false` | Whether to enable speech recognition. After enabling, MaiMai can automatically convert received voice messages to text for processing |

- When `enable_asr` is off, received voice messages will be ignored, `get_voice_text()` directly returns `None` and records warning logs
- After `enable_asr` is enabled, base64 data of voice messages will be sent to the configured audio transcription service through the `voice` task of `LLMServiceClient`
- When transcription fails, error logs will be recorded, and message processing will not be blocked

In the WebUI configuration interface, voice configuration (`VoiceConfig`) belongs to the "Features" category (`__ui_parent__` set to `"emoji"`), in the same group as emoji configuration.

## TTS Configuration in model_config.toml

TTS model selection is implemented through task configuration in `model_config.toml`. The core logic is: declare TTS service API configuration in `[[api_providers]]`, and specify models and API providers for TTS tasks in `[[models]]`.

### Example: Configure GPT-SoVITS

```toml
[[api_providers]]
name = "gpt-sovits"
base_url = "http://127.0.0.1:9880"
api_key = ""                    # GPT-SoVITS local deployment usually doesn't need API Key

[[models]]
name = "tts-gpt-sovits"
api_provider = "gpt-sovits"
model_identifier = "tts"
```

### Example: Configure Doubao TTS

```toml
[[api_providers]]
name = "doubao-tts"
base_url = "https://openspeech.bytedance.com/api/v1"
api_key = "your-api-key"        # Volcano Engine API Key

[[models]]
name = "tts-doubao"
api_provider = "doubao-tts"
model_identifier = "tts"
```

### Example: Configure Qianwen Omni

```toml
[[api_providers]]
name = "qwen-omni"
base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1"
api_key = "your-api-key"        # Alibaba Cloud API Key

[[models]]
name = "tts-qwen"
api_provider = "qwen-omni"
model_identifier = "tts"
```

::: tip Tip
The specific model task assignment method (such as the mapping from TTS task names to models in `model_task_config`) depends on the model routing configuration of the MaiBot version. Please refer to the `[model_task_config]` section in the `model_config.toml` template to understand the current version's task assignment rules.
:::

## Usage Process

1. **Deploy TTS Service**: Choose a TTS backend and complete deployment (GPT-SoVITS needs self-building, Doubao/Qianwen through API calls)
2. **Configure API Provider**: Add TTS service API configuration in `model_config.toml` (`[[api_providers]]`)
3. **Configure Model Routing**: Add TTS models and assign tasks to them in `model_config.toml` (`[[models]]`)
4. **Enable Speech Recognition** (Optional): Set `enable_asr = true` in `bot_config.toml`
5. **Configure ASR Model** (Optional): Configure audio transcription models for `voice` tasks in `model_config.toml`
6. **Restart MaiBot**: Make configuration take effect

## Voice Setting Options

### ASR Related

- `enable_asr`: Controls whether to recognize received voice messages
- ASR uses the `voice` task type of `LLMServiceClient`, needs to configure available audio transcription models for this task in `model_config.toml`
- Voice messages are first converted to base64 encoding, then sent to transcription services via API

### TTS Related

- TTS tasks are called through the task routing mechanism of `LLMServiceClient`
- Audio quality and naturalness of TTS synthesis depend on the selected backend service and voice configuration
- Response latency of TTS services directly affects the reply speed of voice messages

### Database Storage

In the `[database]` section of `bot_config.toml`, the `save_binary_data` configuration item affects the processing method of voice data:

| Configuration Value | Behavior |
|--------------------|----------|
| `false` (default) | Voice and other binary data are deleted after recognition, replaced with recognition results in messages |
| `true` | Voice and other binary data are saved as independent files, replaced with special markers in messages, allowing secondary recognition |

## Notes

- TTS services need to be independently deployed or API enabled, MaiBot itself does not include TTS engines
- Voice message sending is subject to platform restrictions, some platforms may have length or format restrictions on voice messages
- When enabling ASR, ensure the model service corresponding to the `voice` task is available, otherwise voice messages will be skipped
- GPT-SoVITS local deployment requires GPU resources, recommended at least 4GB VRAM
- Doubao TTS and Qianwen Omni are pay-per-use services, please pay attention to usage and costs
- It is recommended to appropriately reduce the frequency of voice message usage in scenarios with high network latency
- ASR service exceptions will not block message processing, only record warning logs
