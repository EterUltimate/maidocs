---
title: Tool 组件
---

# Tool 组件

Tool 组件是 MaiBot 插件系统中最核心的组件类型之一。它允许插件向 LLM 暴露可调用的工具函数，使 LLM 能够在推理过程中主动调用外部能力——例如搜索知识库、查询数据库、调用外部 API 等。

## 核心概念

### ToolInfo 数据类

`ToolInfo` 是 Host 内部使用的工具信息快照，定义在 `src/core/types.py` 中：

```python
@dataclass(slots=True)
class ToolInfo(ComponentInfo):
    parameters_schema: Dict[str, Any] | None = None
    component_type: ComponentType = field(init=False, default=ComponentType.TOOL)
```

它继承自 `ComponentInfo`，包含以下关键字段：

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | `str` | 工具名称，需全局唯一 |
| `description` | `str` | 工具描述，供 LLM 判断是否调用 |
| `parameters_schema` | `Dict[str, Any] \| None` | 对象级工具参数 Schema |
| `enabled` | `bool` | 工具是否启用，默认为 `True` |
| `plugin_name` | `str` | 所属插件 ID |
| `component_type` | `ComponentType` | 固定为 `ComponentType.TOOL` |

### parameters_schema 与 LLM 工具定义

`parameters_schema` 是 JSON Schema 格式的参数描述，用于向 LLM 声明工具需要的参数。例如：

```python
parameters_schema = {
    "type": "object",
    "properties": {
        "query": {
            "type": "string",
            "description": "搜索关键词"
        },
        "limit": {
            "type": "integer",
            "description": "返回结果数量上限",
            "default": 5
        }
    },
    "required": ["query"]
}
```

`ToolInfo.get_llm_definition()` 方法会将 `name`、`description` 和 `parameters_schema` 组合为供 LLM 使用的规范化工具定义字典：

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

## 统一工具抽象层

### ToolSpec

`ToolSpec` 是统一工具声明，定义在 `src/core/tooling.py` 中。它是对 `ToolInfo` 的高阶封装，增加了更丰富的元数据：

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | `str` | 工具名称 |
| `brief_description` | `str` | 简要描述 |
| `detailed_description` | `str` | 详细描述 |
| `parameters_schema` | `Dict[str, Any] \| None` | 参数 Schema |
| `output_schema` | `Dict[str, Any] \| None` | 输出 Schema |
| `provider_name` | `str` | 来源 Provider 名称 |
| `provider_type` | `str` | 来源 Provider 类型 |
| `enabled` | `bool` | 是否启用 |
| `annotation` | `ToolAnnotation \| None` | 工具注解信息 |

`ToolSpec.to_llm_definition()` 方法会生成 `ToolDefinitionInput` 对象，可直接交给模型层使用。

### ToolProvider 协议

`ToolProvider` 是统一工具提供者协议，定义为一个 `Protocol` 类：

```python
@runtime_checkable
class ToolProvider(Protocol):
    provider_name: str
    provider_type: str

    async def list_tools(self) -> list[ToolSpec]: ...
    async def invoke(self, invocation: ToolInvocation, context: Optional[ToolExecutionContext] = None) -> ToolExecutionResult: ...
    async def close(self) -> None: ...
```

任何实现了该协议的类都可以作为工具提供者注册到 `ToolRegistry`。

### ToolRegistry

`ToolRegistry` 是统一工具注册表，负责管理所有工具提供者并提供统一的查询和调用接口：

- **`register_provider(provider)`**：注册工具提供者（同名 Provider 会替换旧的）
- **`list_tools()`**：按 Provider 顺序列出全部去重后的工具（跳过 `enabled=False` 的工具）
- **`get_llm_definitions()`**：获取供 LLM 使用的工具定义列表
- **`invoke(invocation, context)`**：按 Provider 顺序查找目标工具并执行调用

当检测到重复工具名时，会保留先注册的工具并记录警告日志。

## PluginToolProvider

`PluginToolProvider`（定义在 `src/plugin_runtime/tool_provider.py`）是将插件 Tool 暴露为统一工具 Provider 的桥接类：

- `provider_name = "plugin_runtime"`
- `provider_type = "plugin"`
- `list_tools()`：委托给 `component_query_service.get_llm_available_tool_specs()`
- `invoke()`：委托给 `component_query_service.invoke_tool_as_tool()`

它将插件声明式注册的 Tool 组件与 MaiSaka 内部的工具调用管线连接起来。

## 在插件中声明 Tool

在插件的 manifest 中声明 Tool 组件时，需要提供 `name`、`description` 和 `parameters_schema`。MaiBot 会在工具被 LLM 选中时，通过 IPC 调用插件 Runner 中对应的处理函数。

### 调用流程

1. LLM 在推理过程中输出工具调用请求（`tool_call`）
2. Host 根据 `tool_name` 在 `ToolRegistry` 中查找对应的 Provider
3. `PluginToolProvider` 将调用转化为 IPC 消息发送给插件 Runner
4. Runner 执行实际逻辑并返回 `ToolExecutionResult`
5. 结果被写入对话历史，供 LLM 继续推理

## 工具调用相关类型

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

`ToolExecutionResult.get_history_content()` 方法按优先级返回适合写入对话历史的结果文本：文本内容 > 内容项摘要 > 结构化内容 > 错误信息。
