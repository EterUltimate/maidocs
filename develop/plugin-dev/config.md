---
title: 配置管理
---

# 配置管理

MaiBot 插件支持声明式的配置管理机制，允许插件定义自己的配置 Schema，并通过 Host 的 `config` 能力 API 读写配置数据。同时，插件配置可与 WebUI 集成，提供可视化编辑界面。

## 配置文件位置

每个插件的配置文件位于插件目录下的 `config.toml`，路径结构如下：

```
plugins/
└── my_plugin/
    ├── plugin.toml          # 插件 manifest
    ├── config.toml          # 插件配置文件
    └── ...
```

配置文件使用 TOML 格式，与 MaiBot 主程序的配置风格保持一致。

## 配置 Schema

插件可以在 manifest（`plugin.toml`）中通过 `config_schema` 字段声明配置结构。Schema 描述了各配置项的类型、默认值和说明，用于：

- **配置验证**：确保用户提供的配置值符合预期类型
- **WebUI 渲染**：自动生成配置编辑界面
- **默认值填充**：未配置的项自动使用默认值

### Schema 声明示例

```toml
[config_schema]
[config_schema.api_endpoint]
type = "string"
description = "外部 API 地址"
default = "https://api.example.com"

[config_schema.timeout]
type = "integer"
description = "请求超时时间（秒）"
default = 30

[config_schema.debug_mode]
type = "boolean"
description = "是否开启调试模式"
default = false
```

## 配置能力 API

插件通过能力系统访问配置，Host 提供以下配置相关能力：

### config.get — 读取宿主全局配置

读取宿主（MaiBot 主程序）的全局配置中的单个字段。

```python
# 能力调用参数
{
    "key": "bot.name",        # 以点号分隔的配置路径
    "default": "麦麦"          # 可选，未命中时返回的默认值
}
```

**返回值**：

```python
{"success": True, "value": "麦麦"}
```

支持嵌套路径访问，例如 `"emoji.max_reg_num"` 会依次访问 `global_config.emoji.max_reg_num`。

### config.get_plugin — 读取插件配置

读取指定插件的配置数据。若不指定 `plugin_name`，默认读取当前调用者自身的配置。

```python
# 能力调用参数
{
    "plugin_name": "my_plugin",  # 可选，默认为当前插件 ID
    "key": "api_endpoint",       # 可选，读取特定字段；不传则返回全部配置
    "default": "https://fallback.example.com"  # 可选
}
```

**返回值**：

```python
{"success": True, "value": "https://api.example.com"}
```

### config.get_all — 读取插件全部配置

读取指定插件的完整配置字典。

```python
# 能力调用参数
{
    "plugin_name": "my_plugin"  # 可选，默认为当前插件 ID
}
```

**返回值**：

```python
{"success": True, "value": {"api_endpoint": "https://api.example.com", "timeout": 30, "debug_mode": false}}
```

## WebUI 集成

插件的配置 Schema 会在 WebUI 中自动渲染为可视化编辑表单。用户可以在 WebUI 的插件管理页面中直接修改配置，而无需手动编辑 TOML 文件。

### 组件能力支持

Host 还提供了以下与插件配置 Schema 相关的能力，供插件间协同使用：

| 能力名称 | 说明 |
|----------|------|
| `component.get_plugin_config_schema` | 获取指定插件的配置 Schema |
| `component.get_plugin_info` | 获取插件详情（含配置信息） |
| `component.get_all_plugins` | 获取所有插件信息 |

## 配置最佳实践

1. **始终声明 Schema**：即使配置项很少，也应在 manifest 中声明 `config_schema`，确保 WebUI 能正确渲染编辑界面。

2. **提供合理的默认值**：每个配置项都应设置默认值，使插件在无需额外配置的情况下即可正常运行。

3. **使用点号路径访问嵌套配置**：`config.get` 支持以点号分隔的路径访问嵌套对象，避免一次性读取整个配置树。

4. **不要硬编码敏感信息**：API 密钥、密码等敏感字段应通过配置系统管理，而非在代码中硬编码。

5. **配置文件改动只改模版**：遵循项目约定，配置文件的变更应通过模版和版本号控制，不要直接编辑用户的运行时配置文件。
