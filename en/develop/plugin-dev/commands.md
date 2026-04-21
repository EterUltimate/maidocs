---
title: Command Components
---

# Command Components

Command is a regex-based command component in MaiBot's plugin system. When a user's sent message matches a Command's regex pattern, MaiBot schedules the execution of the corresponding Command handler function.

## CommandInfo Data Structure

Each Command is represented on the Host side by the `CommandInfo` data class, inheriting from `ComponentInfo`:

```python
@dataclass(slots=True)
class CommandInfo(ComponentInfo):
    component_type: ComponentType  # Fixed as COMMAND
```

### Fields Inherited from ComponentInfo

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Component name |
| `description` | string | Component description |
| `enabled` | bool | Whether component is enabled, default `True` |
| `plugin_name` | string | Plugin ID it belongs to |

## Command Routing Mechanism

MaiBot's command routing is implemented through `ComponentRegistry`:

1. **Registration Phase**: Plugin declares Command components and their regex patterns in `plugin.py`, Runner registers them to Host's `ComponentRegistry` through RPC
2. **Matching Phase**: When receiving user messages, `PluginRuntimeManager.find_command_by_text()` traverses all Supervisors' `ComponentRegistry`, calls `find_command_by_text()` for regex matching
3. **Execution Phase**: After successful matching, Host routes the call request to the corresponding component in Runner through `invoke_plugin()`

### Command Matching Return Values

When matching succeeds, `find_command_by_text()` returns the following information:

| Field | Description |
|-------|-------------|
| `name` | Command name |
| `full_name` | Command full name (with plugin prefix) |
| `component_type` | Component type (`command`) |
| `plugin_id` | Command's plugin ID |
| `metadata` | Command metadata |
| `enabled` | Whether command is enabled |
| `matched_groups` | Regex named capture results |

## Command Processing Hooks

There are built-in Hooks before and after command execution, allowing plugins to intercept or rewrite command processing flow:

### chat.command.before_execute

Triggered after command matches successfully, before actual execution:

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | object | Current command message's serialized SessionMessage |
| `command_name` | string | Matched command name |
| `plugin_id` | string | Command's plugin ID |
| `matched_groups` | object | Command regex named capture results |

This Hook allows abort (`allow_abort=True`) and parameter rewriting (`allow_kwargs_mutation=True`), default timeout 5000ms. You can:
- Modify `message` to rewrite message content received by command
- Intercept command execution through `abort`

### chat.command.after_execute

Triggered after command execution ends:

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | object | Current command message's serialized SessionMessage |
| `command_name` | string | Command name |
| `result` | — | Command execution result |

This Hook also allows abort and parameter rewriting, default timeout 5000ms. You can:
- Modify command's return result
- Execute additional logic after command execution (like logging, statistics, etc.)

## Component Type Enumeration

MaiBot defines three component types, distinguished by `ComponentType` enumeration:

| Type | Value | Description |
|------|-------|-------------|
| `ACTION` | `"action"` | Action component, for Planner to call |
| `COMMAND` | `"command"` | Command component, based on regex matching |
| `TOOL` | `"tool"` | Tool component, for LLM to call directly |

`CommandInfo`'s `component_type` is fixed as `ComponentType.COMMAND`.

## Development Example

### Register Simple Command

```python
from maibot_plugin_sdk import create_plugin

plugin = create_plugin()

@plugin.command(pattern=r"^/hello(?P<name>.+)?$")
async def hello_command(text, matched_groups, **kwargs):
    name = matched_groups.get("name", "world").strip()
    return True, f"Hello, {name}!", False
```

### Command Return Values

Command handler functions typically return a triple:

1. **Whether to continue main chain processing** (bool): `True` means continue subsequent processing, `False` means abort
2. **Reply text** (string): Text displayed to user after command execution
3. **Whether to reply** (bool): `True` means send as quote reply

### Intercept Commands Through Hook

```python
@plugin.hook_handler(
    hook_name="chat.command.before_execute",
    mode="blocking",
)
async def on_before_command(message, command_name, **kwargs):
    plugin.logger.info(f"Command {command_name} about to execute")
    # Return modified_kwargs can rewrite parameters
    # Return action="abort" can intercept command execution
    return {"action": "continue"}
```

## Differences from Action Components

| Feature | Command | Action |
|---------|---------|--------|
| Trigger Method | User message regex matching | Planner actively selects and calls |
| Caller | User | LLM Planner |
| Applicable Scenarios | Explicit command-style interaction | Intelligent tool calling |
| Activation Control | Determined by regex pattern | Controlled by `ActionActivationType` |

For more information about Action, please refer to [Action Components](./actions.md), for detailed Hook information please refer to [Hook System](./hooks.md).
