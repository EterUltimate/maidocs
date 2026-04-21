---
title: Hook 系统
---

# Hook 系统

Hook 是 MaiBot 插件系统的核心扩展机制。主程序在关键执行点触发命名 Hook，所有订阅该 Hook 的插件处理器按固定顺序调度执行，从而实现消息拦截、改写和观察。

## HookSpec 规格

每个命名 Hook 由 `HookSpec` 定义其行为约束：

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `name` | string | — | Hook 唯一名称（必填） |
| `description` | string | `""` | Hook 功能描述 |
| `parameters_schema` | object | `{}` | 对象级 JSON Schema，描述 Hook 参数模型 |
| `default_timeout_ms` | int | `0` | 默认超时毫秒数，为 `0` 时回退到系统默认值（30 秒） |
| `allow_blocking` | bool | `True` | 是否允许注册阻塞处理器 |
| `allow_observe` | bool | `True` | 是否允许注册观察处理器 |
| `allow_abort` | bool | `True` | 是否允许处理器中止当前 Hook 调用 |
| `allow_kwargs_mutation` | bool | `True` | 是否允许阻塞处理器修改传入参数 |

## 处理器模式

### blocking（阻塞模式）

- 串行执行，可修改 `kwargs`，也可中止本次 Hook 调用
- 适合需要拦截或改写消息的场景
- 返回 `modified_kwargs` 可以更新后续处理器接收的参数
- 返回 `action: "abort"` 可以终止整个 Hook 调用链

### observe（观察模式）

- 后台并发执行，仅允许旁路观察
- 不参与主流程控制，返回的 `modified_kwargs` 和 `abort` 请求会被忽略
- 适合日志记录、数据分析等不影响主流程的场景

## 调度顺序

Hook 处理器按以下规则全局排序：

1. **模式优先**：`blocking` 先于 `observe`
2. **顺序槽位**：`early` → `normal` → `late`
3. **来源优先**：内置插件先于第三方插件
4. **插件 ID**：按字典序排列
5. **处理器名称**：按字典序排列

## 内置 Hook 列表

MaiBot 的各个业务模块通过 `hook_catalog.py` 注册内置 Hook 规格。以下是主要的内置 Hook：

### 聊天消息链

| Hook 名称 | 触发时机 | 允许中止 | 允许改写 |
|-----------|----------|---------|---------|
| `chat.receive.before_process` | 入站消息执行 `process()` 之前 | ✅ | ✅ |
| `chat.receive.after_process` | 入站消息完成轻量预处理后 | ✅ | ✅ |

**`chat.receive.before_process` 参数：**

| 参数 | 类型 | 说明 |
|------|------|------|
| `message` | object | 当前入站消息的序列化 SessionMessage |

**`chat.receive.after_process` 参数：**

| 参数 | 类型 | 说明 |
|------|------|------|
| `message` | object | 已完成预处理的序列化 SessionMessage |

### 命令执行链

| Hook 名称 | 触发时机 | 允许中止 | 允许改写 |
|-----------|----------|---------|---------|
| `chat.command.before_execute` | 命令匹配成功、实际执行前 | ✅ | ✅ |
| `chat.command.after_execute` | 命令执行结束后 | ✅ | ✅ |

**`chat.command.before_execute` 参数：**

| 参数 | 类型 | 说明 |
|------|------|------|
| `message` | object | 当前命令消息的序列化 SessionMessage |
| `command_name` | string | 命中的命令名称 |
| `plugin_id` | string | 命令所属插件 ID |
| `matched_groups` | object | 命令正则命名捕获结果 |

### 发送服务链

| Hook 名称 | 触发时机 | 允许中止 | 允许改写 |
|-----------|----------|---------|---------|
| `send_service.after_build_message` | 出站 SessionMessage 构建完成后 | ✅ | ✅ |
| `send_service.before_send` | 调用 Platform IO 发送前 | ✅ | ✅ |
| `send_service.after_send` | 发送流程结束后 | ✅ | 否 |

**`send_service.before_send` 参数：**

| 参数 | 类型 | 说明 |
|------|------|------|
| `message` | object | 待发送消息的序列化 SessionMessage |
| `typing` | boolean | 是否模拟打字 |
| `set_reply` | boolean | 是否附带引用回复 |
| `reply_message_id` | string | 被引用消息 ID |
| `storage_message` | boolean | 发送成功后是否写库 |
| `show_log` | boolean | 是否输出发送日志 |

### Maisaka 规划器链

| Hook 名称 | 触发时机 | 允许中止 | 允许改写 |
|-----------|----------|---------|---------|
| `maisaka.planner.before_request` | 向模型发起规划请求前 | 否 | ✅ |
| `maisaka.planner.after_response` | 收到模型响应后 | ✅ | ✅ |

**`maisaka.planner.before_request` 参数：**

| 参数 | 类型 | 说明 |
|------|------|------|
| `messages` | array | 即将发给模型的 PromptMessage 列表 |
| `tool_definitions` | array | 当前候选工具定义列表 |
| `selected_history_count` | integer | 选中的上下文消息数量 |
| `built_message_count` | integer | 实际发送给模型的消息数量 |
| `selection_reason` | string | 上下文选择说明 |
| `session_id` | string | 当前会话 ID |

> 注意：`maisaka.planner.before_request` 的 `allow_abort` 为 `False`，不允许通过 abort 中止规划请求。

## Hook 分发机制

`HookDispatcher` 负责 Hook 的调度执行：

1. 收集所有 Supervisor 中订阅了目标 Hook 的处理器
2. 按全局排序规则排列处理器
3. `blocking` 处理器串行执行，每次执行后检查是否有中止请求或参数变更
4. `observe` 处理器在后台并发执行，不影响主流程
5. 每个处理器有独立的超时控制，超时后标记为失败但不影响其他处理器

## 参数 Schema 构建

插件开发时可以使用 `build_object_schema()` 辅助函数构建 Hook 参数模型：

```python
from src.plugin_runtime.hook_schema_utils import build_object_schema

schema = build_object_schema(
    {
        "message": {"type": "object", "description": "消息对象"},
        "stream_id": {"type": "string", "description": "会话 ID"},
    },
    required=["message", "stream_id"],
)
```

该函数会将属性映射自动包装为标准的 `type: "object"` JSON Schema。
