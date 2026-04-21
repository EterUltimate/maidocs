---
title: Hook System
---

# Hook System

Hook is the core extension mechanism of MaiBot's plugin system. The main program triggers named Hooks at key execution points, and all plugin processors subscribed to that Hook are scheduled to execute in a fixed order, thereby achieving message interception, rewriting, and observation.

## HookSpec Specification

Each named Hook is defined by `HookSpec` with its behavioral constraints:

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `name` | string | — | Hook unique name (required) |
| `description` | string | `""` | Hook function description |
| `parameters_schema` | object | `{}` | Object-level JSON Schema describing Hook parameter model |
| `default_timeout_ms` | int | `0` | Default timeout milliseconds, `0` falls back to system default (30 seconds) |
| `allow_blocking` | bool | `True` | Whether to allow registering blocking processors |
| `allow_observe` | bool | `True` | Whether to allow registering observe processors |
| `allow_abort` | bool | `True` | Whether to allow processors to abort current Hook call |
| `allow_kwargs_mutation` | bool | `True` | Whether to allow blocking processors to modify incoming parameters |

## Processor Modes

### blocking (Blocking Mode)

- Serial execution, can modify `kwargs`, can also abort this Hook call
- Suitable for scenarios requiring interception or rewriting messages
- Return `modified_kwargs` can update parameters received by subsequent processors
- Return `action: "abort"` can terminate entire Hook call chain

### observe (Observation Mode)

- Background concurrent execution, only allows bypass observation
- Does not participate in main flow control, returned `modified_kwargs` and `abort` requests are ignored
- Suitable for scenarios like logging, data analysis that don't affect main flow

## Scheduling Order

Hook processors are globally sorted according to the following rules:

1. **Mode priority**: `blocking` before `observe`
2. **Order slots**: `early` → `normal` → `late`
3. **Source priority**: Built-in plugins before third-party plugins
4. **Plugin ID**: Sorted alphabetically
5. **Processor name**: Sorted alphabetically

## Built-in Hook List

MaiBot's various business modules register built-in Hook specifications through `hook_catalog.py`. Here are the main built-in Hooks:

### Chat Message Chain

| Hook Name | Trigger Timing | Allow Abort | Allow Rewrite |
|-----------|---------------|-------------|---------------|
| `chat.receive.before_process` | Before inbound message executes `process()` | ✅ | ✅ |
| `chat.receive.after_process` | After inbound message completes lightweight preprocessing | ✅ | ✅ |

**`chat.receive.before_process` Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | object | Current inbound message's serialized SessionMessage |

**`chat.receive.after_process` Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | object | Serialized SessionMessage that has completed preprocessing |

### Command Execution Chain

| Hook Name | Trigger Timing | Allow Abort | Allow Rewrite |
|-----------|---------------|-------------|---------------|
| `chat.command.before_execute` | After command matches successfully, before actual execution | ✅ | ✅ |
| `chat.command.after_execute` | After command execution ends | ✅ | ✅ |

**`chat.command.before_execute` Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | object | Current command message's serialized SessionMessage |
| `command_name` | string | Matched command name |
| `plugin_id` | string | Command's plugin ID |
| `matched_groups` | object | Command regex named capture results |

### Send Service Chain

| Hook Name | Trigger Timing | Allow Abort | Allow Rewrite |
|-----------|---------------|-------------|---------------|
| `send_service.after_build_message` | After outbound SessionMessage is built | ✅ | ✅ |
| `send_service.before_send` | Before calling Platform IO to send | ✅ | ✅ |
| `send_service.after_send` | After sending process ends | ✅ | No |

**`send_service.before_send` Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | object | Serialized SessionMessage to be sent |
| `typing` | boolean | Whether to simulate typing |
| `set_reply` | boolean | Whether to include quote reply |
| `reply_message_id` | string | Quoted message ID |
| `storage_message` | boolean | Whether to write to database after successful sending |
| `show_log` | boolean | Whether to output sending log |

### Maisaka Planner Chain

| Hook Name | Trigger Timing | Allow Abort | Allow Rewrite |
|-----------|---------------|-------------|---------------|
| `maisaka.planner.before_request` | Before sending planning request to model | No | ✅ |
| `maisaka.planner.after_response` | After receiving model response | ✅ | ✅ |

**`maisaka.planner.before_request` Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `messages` | array | PromptMessage list to be sent to model |
| `tool_definitions` | array | Current candidate tool definition list |
| `selected_history_count` | integer | Selected context message count |
| `built_message_count` | integer | Actual number of messages sent to model |
| `selection_reason` | string | Context selection explanation |
| `session_id` | string | Current session ID |

> Note: `maisaka.planner.before_request`'s `allow_abort` is `False`, does not allow aborting planning request through abort.

## Hook Distribution Mechanism

`HookDispatcher` is responsible for Hook scheduling execution:

1. Collect all processors subscribed to the target Hook in all Supervisors
2. Arrange processors according to global sorting rules
3. `blocking` processors execute serially, check for abort requests or parameter changes after each execution
4. `observe` processors execute concurrently in background, do not affect main flow
5. Each processor has independent timeout control, marked as failed after timeout but does not affect other processors

## Parameter Schema Building

When developing plugins, you can use the `build_object_schema()` helper function to build Hook parameter models:

```python
from src.plugin_runtime.hook_schema_utils import build_object_schema

schema = build_object_schema(
    {
        "message": {"type": "object", "description": "Message object"},
        "stream_id": {"type": "string", "description": "Session ID"},
    },
    required=["message", "stream_id"],
)
```

This function automatically wraps attribute mappings into standard `type: "object"` JSON Schema.
