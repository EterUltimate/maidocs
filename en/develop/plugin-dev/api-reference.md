---
title: API Reference
---

# API Reference

This document lists all available capability APIs for MaiBot plugins. Plugins call these APIs through the Host-Runner IPC mechanism, each API is grouped by namespace, call format is `namespace.method`.

## Message Sending — send

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `send.text` | `(text: str, stream_id: str, typing?: bool, set_reply?: bool, storage_message?: bool) → {success: bool}` | Send text message to specified stream |
| `send.emoji` | `(emoji_base64: str, stream_id: str, storage_message?: bool) → {success: bool}` | Send emoji image to specified stream |
| `send.image` | `(image_base64: str, stream_id: str, storage_message?: bool) → {success: bool}` | Send image to specified stream |
| `send.command` | `(command: str, stream_id: str, display_message?: str, storage_message?: bool) → {success: bool}` | Send command message to specified stream |
| `send.custom` | `(message_type: str, content: Any, stream_id: str, display_message?: str, typing?: bool, storage_message?: bool) → {success: bool}` | Send custom message to specified stream |

All send methods support optional parameters `sync_to_maisaka_history` and `maisaka_source_kind` to control whether to sync to Maisaka history and message source classification.

## Message Query — message

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `message.get_by_time` | `(start_time: float, end_time: float, limit?: int, limit_mode?: str, filter_mai?: bool) → {success: bool, messages: list}` | Query messages by time range |
| `message.get_by_time_in_chat` | `(chat_id: str, start_time: float, end_time: float, limit?: int, filter_mai?: bool, filter_command?: bool) → {success: bool, messages: list}` | Query messages in specified chat by time range |
| `message.get_recent` | `(chat_id: str, hours?: float, limit?: int, filter_mai?: bool) → {success: bool, messages: list}` | Get recent N hours messages in specified chat |
| `message.count_new` | `(chat_id: str, start_time?: float, end_time?: float) → {success: bool, count: int}` | Count new messages in specified chat |
| `message.build_readable` | `(messages?: list, chat_id?: str, replace_bot_name?: bool, timestamp_mode?: str, truncate?: bool) → {success: bool, text: str}` | Build messages into readable text |

## Chat Session — chat

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `chat.get_all_streams` | `(platform?: str) → {success: bool, streams: list}` | Get all chat sessions for specified platform |
| `chat.get_group_streams` | `(platform?: str) → {success: bool, streams: list}` | Get group chat session list |
| `chat.get_private_streams` | `(platform?: str) → {success: bool, streams: list}` | Get private chat session list |
| `chat.get_stream_by_group_id` | `(group_id: str, platform?: str) → {success: bool, stream: dict}` | Get session by QQ group ID |
| `chat.get_stream_by_user_id` | `(user_id: str, platform?: str) → {success: bool, stream: dict}` | Get private chat session by user ID |

Each stream object contains fields: `session_id`, `platform`, `user_id`, `group_id`, `is_group_session`.

## LLM Service — llm

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `llm.generate` | `(prompt: str \| list, model?: str, temperature?: float, max_tokens?: int) → {success: bool, content?: str, reasoning?: str}` | Execute LLM generation without tools |
| `llm.generate_with_tools` | `(prompt: str \| list, tools?: list, model?: str, temperature?: float, max_tokens?: int) → {success: bool, content?: str, tool_calls?: list}` | Execute LLM generation with tools |
| `llm.get_available_models` | `() → {success: bool, models: list}` | Get current available model task list for Host |

The `prompt` parameter supports string (plain text prompt) or message list (multi-turn conversation) format. The `model` parameter specifies model task name, if not specified uses default model.

## Database Operations — database

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `database.query` | `(model_name: str, query_type: str, filters?: dict, data?: dict, limit?: int, order_by?: str, single_result?: bool) → {success: bool, result: Any}` | General database query (supports get/create/update/delete/count) |
| `database.get` | `(model_name: str, filters?: dict, limit?: int, order_by?: str, single_result?: bool) → {success: bool, result: Any}` | Query data records |
| `database.save` | `(model_name: str, data: dict, key_field?: str, key_value?: Any) → {success: bool, result: Any}` | Save data records |
| `database.delete` | `(model_name: str, filters: dict) → {success: bool, result: Any}` | Delete data records (unconditional deletion not allowed) |
| `database.count` | `(model_name: str, filters?: dict) → {success: bool, count: int}` | Count data records |

`model_name` corresponds to data model class names defined in `src/common/database/database_model.py`.

## Configuration Access — config

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `config.get` | `(key: str, default?: Any) → {success: bool, value: Any}` | Read Host global configuration field |
| `config.get_plugin` | `(plugin_name?: str, key?: str, default?: Any) → {success: bool, value: Any}` | Read specified plugin configuration |
| `config.get_all` | `(plugin_name?: str) → {success: bool, value: dict}` | Read all configuration for specified plugin |

## Emoji Operations — emoji

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `emoji.get_by_description` | `(description: str) → {success: bool, emoji: dict}` | Get emoji by description |
| `emoji.get_random` | `(count?: int) → {success: bool, emojis: list}` | Get random emojis |
| `emoji.get_count` | `() → {success: bool, count: int}` | Get total emoji count |
| `emoji.get_emotions` | `() → {success: bool, emotions: list}` | Get all emotion tags |
| `emoji.get_all` | `() → {success: bool, emojis: list}` | Get all emojis |
| `emoji.get_info` | `() → {success: bool, info: dict}` | Get emoji system information |
| `emoji.register` | `(emoji_base64: str) → {success: bool, message: str}` | Register new emoji |
| `emoji.delete` | `(emoji_hash: str) → {success: bool, message: str}` | Delete specified emoji |

## Person Data — person

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `person.get_id` | `(platform: str, user_id: str) → {success: bool, person_id: str}` | Get person internal ID |
| `person.get_value` | `(person_id: str, field_name: str, default?: Any) → {success: bool, value: Any}` | Get person attribute value |
| `person.get_id_by_name` | `(person_name: str) → {success: bool, person_id: str}` | Get person ID by name |

## Logging Capability — logging

Plugins use standard Python `logging` module to output logs in their own Runner processes. The Host side provides `get_logger(name)` utility function through `src/common/logger.py`. Logs default to Simplified Chinese output.

## Other Capabilities

| API | Signature (Simplified) | Description |
|-----|------------------------|-------------|
| `tool.get_definitions` | `() → {success: bool, tools: list}` | Get current available tool definition list |
| `knowledge.search` | `(query: str, limit?: int, mode?: str, chat_id?: str) → {success: bool, content: str}` | Search long-term memory/knowledge base |
| `frequency.get_current_talk_value` | `(chat_id: str) → {success: bool, value: float}` | Get current talk frequency value |
| `frequency.set_adjust` | `(chat_id: str, value: float) → {success: bool}` | Set talk frequency adjustment coefficient |
| `frequency.get_adjust` | `(chat_id: str) → {success: bool, value: float}` | Get talk frequency adjustment coefficient |
| `render.html2png` | `(html: str) → {success: bool, image: str}` | Render HTML to PNG image (base64) |
| `api.call` / `api.get` / `api.list` / `api.replace_dynamic` | — | External API calling and dynamic replacement capabilities
