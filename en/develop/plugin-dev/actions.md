---
title: Action Components
---

# Action Components

Action is the action component in MaiBot's plugin system for Planner (planner) to call. When the Maisaka planner decides to execute an operation, it selects the appropriate Action from the available Action list to execute. Action is exposed to the planner as an LLM-callable tool, enabling it to interact with the external world.

## ActionInfo Data Structure

Each Action is represented on the Host side by the `ActionInfo` data class, inheriting from `ComponentInfo`:

```python
@dataclass(slots=True)
class ActionInfo(ComponentInfo):
    action_parameters: Dict[str, str]       # Action parameters and descriptions
    action_require: List[str]               # Action requirement descriptions
    associated_types: List[str]             # Associated message types
    activation_type: ActionActivationType   # Activation type
    random_activation_probability: float    # Random activation probability
    activation_keywords: List[str]          # Activation keyword list
    keyword_case_sensitive: bool            # Whether keywords are case-sensitive
    parallel_action: bool                   # Whether parallel execution is allowed
    component_type: ComponentType           # Fixed as ACTION
```

### Fields Inherited from ComponentInfo

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Component name |
| `description` | string | Component description |
| `enabled` | bool | Whether component is enabled, default `True` |
| `plugin_name` | string | Plugin ID it belongs to |

## ActionActivationType Activation Types

Action's activation type determines when it will appear in Planner's candidate tool list:

| Type | Value | Description |
|------|-------|-------------|
| `NEVER` | `"never"` | Never activate, default not participating in Planner |
| `ALWAYS` | `"always"` | Default participation in Planner, always as candidate tool |
| `RANDOM` | `"random"` | Randomly enable to Planner with certain probability |
| `KEYWORD` | `"keyword"` | Only enable to Planner when message contains specified keywords |

### Activation Types in Detail

#### ALWAYS (Always Activate)

The most commonly used activation type. Action always appears in Planner's candidate tool list, and the planner can call it at any time.

#### NEVER (Never Activate)

Action does not participate in Planner's tool selection. Suitable for scenarios requiring manual enablement or on-demand activation through Hooks.

#### RANDOM (Random Activation)

Decide whether to add Action to current round's candidate list with probability set by `random_activation_probability` (0.0 ~ 1.0). Suitable for scenarios where you want Action to appear irregularly and increase interaction diversity.

#### KEYWORD (Keyword Activation)

Action is only added to candidate list when user message contains keywords in `activation_keywords` list. `keyword_case_sensitive` controls whether keyword matching is case-sensitive. Suitable for Actions only needed in specific topics.

## Field Details

### action_parameters

Dictionary of action parameters and descriptions, format is `{"parameter_name": "parameter_description"}`. These descriptions are provided to LLM as part of tool schema, helping the planner understand the purpose of each parameter.

```python
action_parameters = {
    "query": "Search keywords",
    "count": "Number of results to return"
}
```

### action_require

List of action requirement descriptions, used to declare prerequisites or resources required for this Action's execution. This information helps the Host side determine whether the Action can be executed.

```python
action_require = ["Requires network connection", "Requires user authorization"]
```

### associated_types

List of associated message types, used to limit Action to only be triggered in specific message type contexts. For example, some Actions only handle group messages, some only handle private messages.

### parallel_action

Boolean value indicating whether this Action allows parallel execution with other Actions. When set to `True`, Planner can call this Action and other parallel-safe Actions simultaneously in the same planning round, thereby improving execution efficiency.

## How Action Becomes Planner Tool

1. Plugin registers Action components through `create_plugin()` in `plugin.py`
2. Runner registers Action declarations to Host's `ComponentRegistry` through RPC
3. Host converts `ActionInfo` to tool definitions recognizable by Planner
4. Decide whether Action joins current round's candidate tool list based on `activation_type`
5. Planner (Maisaka) selects appropriate Action to call from candidate list during planning
6. Host routes call request to Runner execution through `invoke_plugin()`

## Development Example

```python
from maibot_plugin_sdk import create_plugin

plugin = create_plugin()

@plugin.action(
    name="search",
    description="Search the internet for information",
    action_parameters={"query": "Search keywords"},
    activation_type="always",
)
async def search_action(query: str, **kwargs):
    # Execute search logic
    result = await do_search(query)
    return result
```

For more component development details, please refer to [Plugin Development Guide](./index.md) and [Command Components](./commands.md).
