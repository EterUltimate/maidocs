---
title: Command 组件
---

# Command 组件

Command 是 MaiBot 插件系统中基于正则匹配的命令组件。当用户发送的消息匹配到某个 Command 的正则模式时，MaiBot 会调度执行对应的 Command 处理函数。

## CommandInfo 数据结构

每个 Command 在 Host 侧以 `CommandInfo` 数据类表示，继承自 `ComponentInfo`：

```python
@dataclass(slots=True)
class CommandInfo(ComponentInfo):
    component_type: ComponentType  # 固定为 COMMAND
```

### 继承自 ComponentInfo 的字段

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | string | 组件名称 |
| `description` | string | 组件描述 |
| `enabled` | bool | 组件是否启用，默认 `True` |
| `plugin_name` | string | 所属插件 ID |

## 命令路由机制

MaiBot 的命令路由通过 `ComponentRegistry` 实现：

1. **注册阶段**：插件在 `plugin.py` 中声明 Command 组件及其正则模式，Runner 通过 RPC 将其注册到 Host 的 `ComponentRegistry`
2. **匹配阶段**：当收到用户消息时，`PluginRuntimeManager.find_command_by_text()` 遍历所有 Supervisor 的 `ComponentRegistry`，调用 `find_command_by_text()` 进行正则匹配
3. **执行阶段**：匹配成功后，Host 通过 `invoke_plugin()` 将调用请求路由到 Runner 中的对应组件

### 命令匹配返回值

匹配成功时，`find_command_by_text()` 返回以下信息：

| 字段 | 说明 |
|------|------|
| `name` | 命令名称 |
| `full_name` | 命令完整名称（含插件前缀） |
| `component_type` | 组件类型（`command`） |
| `plugin_id` | 命令所属插件 ID |
| `metadata` | 命令元数据 |
| `enabled` | 命令是否启用 |
| `matched_groups` | 正则命名捕获结果 |

## 命令处理 Hook

命令的执行前后各有一个内置 Hook，允许插件拦截或改写命令处理流程：

### chat.command.before_execute

在命令匹配成功、实际执行前触发：

| 参数 | 类型 | 说明 |
|------|------|------|
| `message` | object | 当前命令消息的序列化 SessionMessage |
| `command_name` | string | 命中的命令名称 |
| `plugin_id` | string | 命令所属插件 ID |
| `matched_groups` | object | 命令正则命名捕获结果 |

此 Hook 允许中止（`allow_abort=True`）和改写参数（`allow_kwargs_mutation=True`），默认超时 5000ms。你可以：
- 修改 `message` 来改写命令收到的消息内容
- 通过 `abort` 拦截命令执行

### chat.command.after_execute

在命令执行结束后触发：

| 参数 | 类型 | 说明 |
|------|------|------|
| `message` | object | 当前命令消息的序列化 SessionMessage |
| `command_name` | string | 命令名称 |
| `result` | — | 命令执行结果 |

此 Hook 同样允许中止和改写参数，默认超时 5000ms。你可以：
- 修改命令的返回结果
- 在命令执行后执行额外的逻辑（如日志记录、统计等）

## 组件类型枚举

MaiBot 定义了三种组件类型，通过 `ComponentType` 枚举区分：

| 类型 | 值 | 说明 |
|------|-----|------|
| `ACTION` | `"action"` | 动作组件，供 Planner 调用 |
| `COMMAND` | `"command"` | 命令组件，基于正则匹配 |
| `TOOL` | `"tool"` | 工具组件，供 LLM 直接调用 |

`CommandInfo` 的 `component_type` 固定为 `ComponentType.COMMAND`。

## 开发示例

### 注册简单命令

```python
from maibot_plugin_sdk import create_plugin

plugin = create_plugin()

@plugin.command(pattern=r"^/hello(?P<name>.+)?$")
async def hello_command(text, matched_groups, **kwargs):
    name = matched_groups.get("name", "世界").strip()
    return True, f"你好，{name}！", False
```

### 命令返回值

Command 处理函数通常返回一个三元组：

1. **是否继续主链处理**（bool）：`True` 表示继续后续处理，`False` 表示中止
2. **回复文本**（string）：命令执行后展示给用户的文本
3. **是否需要回复**（bool）：`True` 表示以引用回复的形式发送

### 通过 Hook 拦截命令

```python
@plugin.hook_handler(
    hook_name="chat.command.before_execute",
    mode="blocking",
)
async def on_before_command(message, command_name, **kwargs):
    plugin.logger.info(f"命令 {command_name} 即将执行")
    # 返回 modified_kwargs 可以改写参数
    # 返回 action="abort" 可以拦截命令执行
    return {"action": "continue"}
```

## 与 Action 组件的区别

| 特性 | Command | Action |
|------|---------|--------|
| 触发方式 | 用户消息正则匹配 | Planner 主动选择调用 |
| 调用者 | 用户 | LLM 规划器 |
| 适用场景 | 明确的命令式交互 | 智能化的工具调用 |
| 激活控制 | 由正则模式决定 | 由 `ActionActivationType` 控制 |

更多关于 Action 的信息请参考 [Action 组件](./actions.md)，关于 Hook 的详细信息请参考 [Hook 系统](./hooks.md)。
