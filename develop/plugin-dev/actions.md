---
title: Action 组件
---

# Action 组件

Action 是 MaiBot 插件系统中供 Planner（规划器）调用的动作组件。当 Maisaka 规划器决定执行某项操作时，会从可用的 Action 列表中选择合适的 Action 来执行。Action 作为 LLM 可调用的工具暴露给规划器，使其能够与外部世界交互。

## ActionInfo 数据结构

每个 Action 在 Host 侧以 `ActionInfo` 数据类表示，继承自 `ComponentInfo`：

```python
@dataclass(slots=True)
class ActionInfo(ComponentInfo):
    action_parameters: Dict[str, str]       # 动作参数与描述
    action_require: List[str]               # 动作需求说明
    associated_types: List[str]             # 关联的消息类型
    activation_type: ActionActivationType   # 激活类型
    random_activation_probability: float    # 随机激活概率
    activation_keywords: List[str]          # 激活关键词列表
    keyword_case_sensitive: bool            # 关键词是否区分大小写
    parallel_action: bool                   # 是否允许并行执行
    component_type: ComponentType           # 固定为 ACTION
```

### 继承自 ComponentInfo 的字段

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | string | 组件名称 |
| `description` | string | 组件描述 |
| `enabled` | bool | 组件是否启用，默认 `True` |
| `plugin_name` | string | 所属插件 ID |

## ActionActivationType 激活类型

Action 的激活类型决定了它何时会出现在 Planner 的候选工具列表中：

| 类型 | 值 | 说明 |
|------|-----|------|
| `NEVER` | `"never"` | 从不激活，默认不参与到 Planner 中 |
| `ALWAYS` | `"always"` | 默认参与 Planner，始终作为候选工具 |
| `RANDOM` | `"random"` | 以一定概率随机启用到 Planner 中 |
| `KEYWORD` | `"keyword"` | 当消息中包含指定关键词时才启用到 Planner 中 |

### 各激活类型详解

#### ALWAYS（始终激活）

最常用的激活类型。Action 始终出现在 Planner 的候选工具列表中，规划器可以随时选择调用它。

#### NEVER（从不激活）

Action 不参与 Planner 的工具选择。适用于需要手动启用或通过 Hook 按需激活的场景。

#### RANDOM（随机激活）

以 `random_activation_probability` 设置的概率（0.0 ~ 1.0）决定是否将 Action 加入当前轮次的候选列表。适用于希望 Action 不定期出现、增加交互多样性的场景。

#### KEYWORD（关键词激活）

当用户消息中包含 `activation_keywords` 列表中的关键词时，Action 才会被加入候选列表。`keyword_case_sensitive` 控制关键词匹配是否区分大小写。适用于只在特定话题下才需要用到的 Action。

## 字段详解

### action_parameters

动作参数与描述的字典，格式为 `{"参数名": "参数描述"}`。这些描述会作为工具 Schema 的一部分提供给 LLM，帮助规划器理解每个参数的用途。

```python
action_parameters = {
    "query": "搜索关键词",
    "count": "返回结果数量"
}
```

### action_require

动作需求说明列表，用于声明该 Action 执行所需的前置条件或资源。这些信息帮助 Host 侧判断 Action 是否可执行。

```python
action_require = ["需要网络连接", "需要用户授权"]
```

### associated_types

关联的消息类型列表，用于限定 Action 只在特定类型的消息上下文中被触发。例如某些 Action 只处理群组消息，某些只处理私聊消息。

### parallel_action

布尔值，表示该 Action 是否允许与其他 Action 同时并行执行。当设为 `True` 时，Planner 可以在同一轮规划中同时调用此 Action 和其他并行安全的 Action，从而提高执行效率。

## Action 如何成为 Planner 工具

1. 插件在 `plugin.py` 中通过 `create_plugin()` 注册 Action 组件
2. Runner 将 Action 声明通过 RPC 注册到 Host 的 `ComponentRegistry`
3. Host 将 `ActionInfo` 转换为 Planner 可识别的工具定义
4. 根据 `activation_type` 决定 Action 是否加入当前轮次的候选工具列表
5. Planner（Maisaka）在规划时从候选列表中选择合适的 Action 调用
6. Host 通过 `invoke_plugin()` 将调用请求路由到 Runner 执行

## 开发示例

```python
from maibot_plugin_sdk import create_plugin

plugin = create_plugin()

@plugin.action(
    name="search",
    description="搜索互联网获取信息",
    action_parameters={"query": "搜索关键词"},
    activation_type="always",
)
async def search_action(query: str, **kwargs):
    # 执行搜索逻辑
    result = await do_search(query)
    return result
```

更多组件开发细节请参考 [插件开发指南](./index.md) 和 [Command 组件](./commands.md)。
