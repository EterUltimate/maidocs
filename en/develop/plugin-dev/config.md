---
title: Configuration
---

# Configuration

MaiBot plugins support declarative configuration management, allowing plugins to define their own configuration Schema and read/write configuration data through Host's `config` capability API. At the same time, plugin configuration can integrate with WebUI to provide visual editing interface.

## Configuration File Location

Each plugin's configuration file is located at `config.toml` under the plugin directory, with the following path structure:

```
plugins/
└── my_plugin/
    ├── plugin.toml          # Plugin manifest
    ├── config.toml          # Plugin configuration file
    └── ...
```

Configuration files use TOML format, consistent with MaiBot main program's configuration style.

## Configuration Schema

Plugins can declare configuration structure through the `config_schema` field in manifest (`plugin.toml`). Schema describes the type, default value, and description of each configuration item, used for:

- **Configuration validation**: Ensure user-provided configuration values match expected types
- **WebUI rendering**: Automatically generate configuration editing interface
- **Default value filling**: Automatically use default values for unconfigured items

### Schema Declaration Example

```toml
[config_schema]
[config_schema.api_endpoint]
type = "string"
description = "External API address"
default = "https://api.example.com"

[config_schema.timeout]
type = "integer"
description = "Request timeout (seconds)"
default = 30

[config_schema.debug_mode]
type = "boolean"
description = "Whether to enable debug mode"
default = false
```

## Configuration Capability API

Plugins access configuration through the capability system, Host provides the following configuration-related capabilities:

### config.get — Read Host Global Configuration

Read a single field from Host (MaiBot main program) global configuration.

```python
# Capability call parameters
{
    "key": "bot.name",        # Dot-separated configuration path
    "default": "麦麦"          # Optional, default value when not found
}
```

**Return value**:

```python
{"success": True, "value": "麦麦"}
```

Supports nested path access, for example `"emoji.max_reg_num"` will sequentially access `global_config.emoji.max_reg_num`.

### config.get_plugin — Read Plugin Configuration

Read configuration data for specified plugin. If `plugin_name` is not specified, it defaults to reading the current caller's own configuration.

```python
# Capability call parameters
{
    "plugin_name": "my_plugin",  # Optional, defaults to current plugin ID
    "key": "api_endpoint",       # Optional, read specific field; omit to return all configuration
    "default": "https://fallback.example.com"  # Optional
}
```

**Return value**:

```python
{"success": True, "value": "https://api.example.com"}
```

### config.get_all — Read Plugin All Configuration

Read complete configuration dictionary for specified plugin.

```python
# Capability call parameters
{
    "plugin_name": "my_plugin"  # Optional, defaults to current plugin ID
}
```

**Return value**:

```python
{"success": True, "value": {"api_endpoint": "https://api.example.com", "timeout": 30, "debug_mode": false}}
```

## WebUI Integration

Plugin configuration Schema will be automatically rendered as visual editing forms in WebUI. Users can directly modify configuration in WebUI's plugin management page without manually editing TOML files.

### Component Capability Support

Host also provides the following capabilities related to plugin configuration Schema for inter-plugin collaboration:

| Capability Name | Description |
|-----------------|-------------|
| `component.get_plugin_config_schema` | Get configuration Schema for specified plugin |
| `component.get_plugin_info` | Get plugin details (including configuration information) |
| `component.get_all_plugins` | Get information for all plugins |

## Configuration Best Practices

1. **Always declare Schema**: Even with few configuration items, declare `config_schema` in manifest to ensure WebUI can correctly render editing interface.

2. **Provide reasonable default values**: Each configuration item should have default values so plugins can run normally without additional configuration.

3. **Use dot path to access nested configuration**: `config.get` supports dot-separated path access to nested objects, avoiding reading entire configuration tree at once.

4. **Don't hard-code sensitive information**: Sensitive fields like API keys, passwords should be managed through configuration system rather than hard-coded in code.

5. **Only modify template for configuration file changes**: Follow project conventions, configuration file changes should be controlled through templates and version numbers, don't directly edit user's runtime configuration files.
