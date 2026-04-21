---
title: API 参考
---

# API 参考

本文档列出 MaiBot 插件可用的全部能力 API。插件通过 Host-Runner IPC 机制调用这些 API，每个 API 按命名空间分组，调用格式为 `namespace.method`。

## 消息发送 — send

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `send.text` | `(text: str, stream_id: str, typing?: bool, set_reply?: bool, storage_message?: bool) → {success: bool}` | 向指定流发送文本消息 |
| `send.emoji` | `(emoji_base64: str, stream_id: str, storage_message?: bool) → {success: bool}` | 向指定流发送表情图片 |
| `send.image` | `(image_base64: str, stream_id: str, storage_message?: bool) → {success: bool}` | 向指定流发送图片 |
| `send.command` | `(command: str, stream_id: str, display_message?: str, storage_message?: bool) → {success: bool}` | 向指定流发送命令消息 |
| `send.custom` | `(message_type: str, content: Any, stream_id: str, display_message?: str, typing?: bool, storage_message?: bool) → {success: bool}` | 向指定流发送自定义消息 |

所有 send 方法均支持 `sync_to_maisaka_history` 和 `maisaka_source_kind` 可选参数，用于控制是否同步到麦麦历史记录和消息来源分类。

## 消息查询 — message

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `message.get_by_time` | `(start_time: float, end_time: float, limit?: int, limit_mode?: str, filter_mai?: bool) → {success: bool, messages: list}` | 按时间范围查询消息 |
| `message.get_by_time_in_chat` | `(chat_id: str, start_time: float, end_time: float, limit?: int, filter_mai?: bool, filter_command?: bool) → {success: bool, messages: list}` | 按时间范围查询指定聊天的消息 |
| `message.get_recent` | `(chat_id: str, hours?: float, limit?: int, filter_mai?: bool) → {success: bool, messages: list}` | 获取指定聊天最近 N 小时的消息 |
| `message.count_new` | `(chat_id: str, start_time?: float, end_time?: float) → {success: bool, count: int}` | 统计指定聊天的新消息数量 |
| `message.build_readable` | `(messages?: list, chat_id?: str, replace_bot_name?: bool, timestamp_mode?: str, truncate?: bool) → {success: bool, text: str}` | 将消息构建为可读文本 |

## 聊天会话 — chat

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `chat.get_all_streams` | `(platform?: str) → {success: bool, streams: list}` | 获取指定平台全部聊天会话 |
| `chat.get_group_streams` | `(platform?: str) → {success: bool, streams: list}` | 获取群聊会话列表 |
| `chat.get_private_streams` | `(platform?: str) → {success: bool, streams: list}` | 获取私聊会话列表 |
| `chat.get_stream_by_group_id` | `(group_id: str, platform?: str) → {success: bool, stream: dict}` | 按 QQ 群号获取会话 |
| `chat.get_stream_by_user_id` | `(user_id: str, platform?: str) → {success: bool, stream: dict}` | 按用户 ID 获取私聊会话 |

每个 stream 对象包含 `session_id`、`platform`、`user_id`、`group_id`、`is_group_session` 字段。

## LLM 服务 — llm

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `llm.generate` | `(prompt: str \| list, model?: str, temperature?: float, max_tokens?: int) → {success: bool, content?: str, reasoning?: str}` | 执行无工具的 LLM 生成 |
| `llm.generate_with_tools` | `(prompt: str \| list, tools?: list, model?: str, temperature?: float, max_tokens?: int) → {success: bool, content?: str, tool_calls?: list}` | 执行带工具的 LLM 生成 |
| `llm.get_available_models` | `() → {success: bool, models: list}` | 获取当前宿主可用的模型任务列表 |

`prompt` 参数支持字符串（纯文本提示）或消息列表（多轮对话）格式。`model` 参数指定模型任务名称，若不指定则使用默认模型。

## 数据库操作 — database

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `database.query` | `(model_name: str, query_type: str, filters?: dict, data?: dict, limit?: int, order_by?: str, single_result?: bool) → {success: bool, result: Any}` | 通用数据库查询（支持 get/create/update/delete/count） |
| `database.get` | `(model_name: str, filters?: dict, limit?: int, order_by?: str, single_result?: bool) → {success: bool, result: Any}` | 查询数据记录 |
| `database.save` | `(model_name: str, data: dict, key_field?: str, key_value?: Any) → {success: bool, result: Any}` | 保存数据记录 |
| `database.delete` | `(model_name: str, filters: dict) → {success: bool, result: Any}` | 删除数据记录（不允许无条件删除） |
| `database.count` | `(model_name: str, filters?: dict) → {success: bool, count: int}` | 统计数据记录数量 |

`model_name` 对应 `src/common/database/database_model.py` 中定义的数据模型类名。

## 配置访问 — config

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `config.get` | `(key: str, default?: Any) → {success: bool, value: Any}` | 读取宿主全局配置字段 |
| `config.get_plugin` | `(plugin_name?: str, key?: str, default?: Any) → {success: bool, value: Any}` | 读取指定插件配置 |
| `config.get_all` | `(plugin_name?: str) → {success: bool, value: dict}` | 读取指定插件全部配置 |

## 表情操作 — emoji

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `emoji.get_by_description` | `(description: str) → {success: bool, emoji: dict}` | 按描述获取表情包 |
| `emoji.get_random` | `(count?: int) → {success: bool, emojis: list}` | 随机获取表情包 |
| `emoji.get_count` | `() → {success: bool, count: int}` | 获取表情包总数 |
| `emoji.get_emotions` | `() → {success: bool, emotions: list}` | 获取全部情绪标签 |
| `emoji.get_all` | `() → {success: bool, emojis: list}` | 获取全部表情包 |
| `emoji.get_info` | `() → {success: bool, info: dict}` | 获取表情包系统信息 |
| `emoji.register` | `(emoji_base64: str) → {success: bool, message: str}` | 注册新表情包 |
| `emoji.delete` | `(emoji_hash: str) → {success: bool, message: str}` | 删除指定表情包 |

## 人员数据 — person

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `person.get_id` | `(platform: str, user_id: str) → {success: bool, person_id: str}` | 获取人员内部 ID |
| `person.get_value` | `(person_id: str, field_name: str, default?: Any) → {success: bool, value: Any}` | 获取人员属性值 |
| `person.get_id_by_name` | `(person_name: str) → {success: bool, person_id: str}` | 按名称获取人员 ID |

## 日志能力 — logging

插件在自己的 Runner 进程中使用标准 Python `logging` 模块输出日志。Host 侧通过 `src/common/logger.py` 提供 `get_logger(name)` 工具函数。日志默认使用简体中文输出。

## 其他能力

| API | 签名（简化） | 说明 |
|-----|-------------|------|
| `tool.get_definitions` | `() → {success: bool, tools: list}` | 获取当前可用工具定义列表 |
| `knowledge.search` | `(query: str, limit?: int, mode?: str, chat_id?: str) → {success: bool, content: str}` | 搜索长期记忆/知识库 |
| `frequency.get_current_talk_value` | `(chat_id: str) → {success: bool, value: float}` | 获取当前说话频率值 |
| `frequency.set_adjust` | `(chat_id: str, value: float) → {success: bool}` | 设置说话频率调整系数 |
| `frequency.get_adjust` | `(chat_id: str) → {success: bool, value: float}` | 获取说话频率调整系数 |
| `render.html2png` | `(html: str) → {success: bool, image: str}` | 将 HTML 渲染为 PNG 图片（base64） |
| `api.call` / `api.get` / `api.list` / `api.replace_dynamic` | — | 外部 API 调用与动态替换能力 |
