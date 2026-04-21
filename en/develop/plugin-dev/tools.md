---
title: Tool Components
---

# Tool Components

Tool component is one of the most core component types in MaiBot's plugin system. It allows plugins to expose callable tool functions to LLM, enabling LLM to actively call external capabilities during reasoning - such as searching knowledge bases, querying databases, calling external APIs, etc.

## Core Concepts

### ToolInfo Data Class

`ToolInfo` is the tool information snapshot used internally by Host, defined in `src/core/types.py`:

```python
@dataclass(slots=True)
class ToolInfo(ComponentInfo):
    parameters_schema: Dict[str, Any] | None = None
    component_type: ComponentType = field(init=False, default=ComponentType.TOOL)
```

It inherits from `ComponentInfo` and contains the following key fields:

| Field | Type | Description |
|-------|------|-------------|
| `name` | `str` | Tool name, must be globally unique |
| `description` | `str` | Tool description, for LLM to decide whether to call |
| `parameters_schema` | `Dict[str, Any] \| None` | Object-level tool parameter Schema |
| `enabled` | `bool` | Whether tool is enabled, default `True` |
| `plugin_name` | `str` | Plugin ID it belongs to |
| `component_type` | `ComponentType` | Fixed as `ComponentType.TOOL` |

### parameters_schema and LLM Tool Definition

`parameters_schema` is parameter description in JSON Schema format, used to declare parameters required by the tool to LLM. For example:

```python
parameters_schema = {
    "type": "object",
    "properties": {
        "query": {
            "type": "string",
            "description": "Search keywords"
        },
        "limit": {
            "type": "integer",
            "description": "Maximum number of results to return",
            "default": 5
        }
    },
    "required": ["query"]
}
```

The `ToolInfo.get_llm_definition()` method combines `name`, `description`, and `parameters_schema` into a normalized tool definition dictionary for LLM use:

```python
def get_llm_definition(self) -> Dict[str, Any]:
    definition = {
        "name": self.name,
        "description": self.description,
    }
    if self.parameters_schema is not None:
        definition["parameters_schema"] = copy.deepcopy(self.parameters_schema)
    return definition
```

## Unified Tool Abstraction Layer

### ToolSpec

`ToolSpec` is the unified tool declaration, defined in `src/core/tooling.py`. It is a higher-level encapsulation of `ToolInfo`, adding richer metadata:

| Field | Type | Description |
|-------|------|-------------|
| `name` | `str` | Tool name |
| `brief_description` | `str` | Brief description |
| `detailed_description` | `str` | Detailed description |
| `parameters_schema` | `Dict[str, Any] \| None` | Parameter Schema |
| `output_schema` | `Dict[str, Any] \| None` | Output Schema |
| `provider_name` | `str` | Source Provider name |
| `provider_type` | `str` | Source Provider type |
| `enabled` | `bool` | Whether enabled |
| `annotation` | `ToolAnnotation \| None` | Tool annotation information |

The `ToolSpec.to_llm_definition()` method generates a `ToolDefinitionInput` object that can be directly used by the model layer.

### ToolProvider Protocol

`ToolProvider` is the unified tool provider protocol, defined as a `Protocol` class:

```python
@runtime_checkable
class ToolProvider(Protocol):
    provider_name: str
    provider_type: str

    async def list_tools(self) -> list[ToolSpec]: ...
    async def invoke(self, invocation: ToolInvocation, context: Optional[ToolExecutionContext] = None) -> ToolExecutionResult: ...
    async def close(self) -> None: ...
```

Any class implementing this protocol can be registered as a tool provider to `ToolRegistry`.

### ToolRegistry

`ToolRegistry` is the unified tool registry, responsible for managing all tool providers and providing unified query and invocation interfaces:

- **`register_provider(provider)`**: Register tool provider (same name Provider will replace old one)
- **`list_tools()`**: List all deduplicated tools in Provider order (skip tools with `enabled=False`)
- **`get_llm_definitions()`**: Get tool definition list for LLM use
- **`invoke(invocation, context)`**: Find target tool in Provider order and execute invocation

When duplicate tool names are detected, it will keep the first registered tool and log warning.

## PluginToolProvider

`PluginToolProvider` (defined in `src/plugin_runtime/tool_provider.py`) is the bridge class that exposes plugin Tools as unified tool Providers:

- `provider_name = "plugin_runtime"`
- `provider_type = "plugin"`
- `list_tools()`: Delegates to `component_query_service.get_llm_available_tool_specs()`
- `invoke()`: Delegates to `component_query_service.invoke_tool_as_tool()`

It connects plugin declaratively registered Tool components with MaiSaka's internal tool invocation pipeline.

## Declaring Tools in Plugins

When declaring Tool components in plugin manifest, you need to provide `name`, `description`, and `parameters_schema`. MaiBot will call the corresponding handler function in plugin Runner through IPC when the tool is selected by LLM.

### Invocation Flow

1. LLM outputs tool call request (`tool_call`) during reasoning process
2. Host finds corresponding Provider in `ToolRegistry` based on `tool_name`
3. `PluginToolProvider` converts the call to IPC message and sends to plugin Runner
4. Runner executes actual logic and returns `ToolExecutionResult`
5. Result is written to conversation history for LLM to continue reasoning

## Tool Invocation Related Types

### ToolInvocation

```python
@dataclass(slots=True)
class ToolInvocation:
    tool_name: str
    arguments: Dict[str, Any] = field(default_factory=dict)
    call_id: str = ""
    session_id: str = ""
    stream_id: str = ""
```

### ToolExecutionResult

```python
@dataclass(slots=True)
class ToolExecutionResult:
    tool_name: str
    success: bool
    content: str = ""
    error_message: str = ""
    structured_content: Any = None
    content_items: list[ToolContentItem] = field(default_factory=list)
```

The `ToolExecutionResult.get_history_content()` method returns result text suitable for writing to conversation history by priority: text content > content item summary > structured content > error information.
