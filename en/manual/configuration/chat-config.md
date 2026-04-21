---
title: Chat Configuration
---

# Chat Configuration

The `[chat]` section and `[message_receive]` section in `bot_config.toml` control MaiBot's chat behavior, speaking frequency, and message processing methods.

## Chat Configuration [chat]

```toml
[chat]
talk_value = 1.0
mentioned_bot_reply = false
inevitable_at_reply = true
max_context_size = 30
planner_interrupt_max_consecutive_count = 2
group_chat_prompt = "..."
private_chat_prompts = "..."
chat_prompts = []
enable_talk_value_rules = true

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "00:00-08:59"
value = 0.8

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "09:00-18:59"
value = 1.0
```

### talk_value

- **Type**: `float`
- **Default**: `1.0`
- **Range**: `0.0 - 1.0`
- **WebUI Control**: Slider, step 0.1
- **Description**: Chat frequency control. Smaller value means MaiBot is more silent; larger value means more active speaking

This is the core parameter controlling MaiBot's speaking frequency:

| Value | Effect |
|----|------|
| `0.0` | Almost never speaks proactively |
| `0.5` | Moderate frequency |
| `1.0` | Active speaking (default) |

::: tip
`talk_value` is the global base probability. Combined with `enable_talk_value_rules`, it can be dynamically adjusted by time period, group/private chat scenarios, etc.
:::

### mentioned_bot_reply

- **Type**: `bool`
- **Default**: `false`
- **Description**: Whether to automatically reply when the robot name is mentioned in the message

When enabled, when users mention the robot's nickname or alias in group chat (not @), MaiBot will also reply.

### inevitable_at_reply

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to definitely reply when receiving @ messages

When enabled, when someone directly @ the robot, MaiBot will definitely reply, unaffected by `talk_value`.

### max_context_size

- **Type**: `int`
- **Default**: `30`
- **Description**: Upper limit of context message count sent to LLM

MaiBot will keep the most recent N messages as context. Larger values can retain more conversation history, but will also consume more tokens.

### planner_interrupt_max_consecutive_count

- **Type**: `int`
- **Default**: `2`
- **Range**: `≥ 0`
- **Description**: Maximum number of consecutive times Planner is interrupted by new messages

When MaiBot is generating a reply, new message arrival may trigger Planner to re-plan. If Planner is interrupted consecutively more than this number, it will force execution of the current plan and reply.

- `0`: Disable interruption protection
- `2`: Force reply after being interrupted 2 times consecutently (default)

### group_chat_prompt

- **Type**: `str`
- **Default**: A group chat behavior guide
- **Description**: Additional attention prompt for group chat scenarios

This prompt will be appended to the system prompt in group chat scenarios, guiding LLM's group chat behavior. The default value includes guidance like "reply briefly", "control frequency", etc.

### private_chat_prompts

- **Type**: `str`
- **Default**: A private chat behavior guide
- **Description**: Additional attention prompt for private chat scenarios

This prompt will be appended to the system prompt in private chat scenarios. Compared with group chat, the default guidance for private chat focuses more on the natural feeling of one-on-one communication.

### chat_prompts

- **Type**: `list[ExtraPromptItem]`
- **Default**: `[]`
- **Description**: Additional prompts for specific platforms or chats

Each `ExtraPromptItem` contains the following fields:

| Field | Type | Description |
|------|------|------|
| `platform` | `str` | Platform identifier (invalid if left empty with `item_id`) |
| `item_id` | `str` | User/Group ID (invalid if left empty with `platform`) |
| `rule_type` | `"group"` \| `"private"` | Chat flow type |
| `prompt` | `str` | Additional prompt content |

::: warning
The three fields `platform`, `item_id`, and `prompt` of `ExtraPromptItem` must all be filled in, otherwise the entry is invalid.
:::

### enable_talk_value_rules

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to enable dynamic speaking frequency rules

When enabled, MaiBot will dynamically adjust speaking frequency by time period and chat flow type according to `talk_value_rules` configuration.

### talk_value_rules

- **Type**: `list[TalkRulesItem]`
- **Default**: Contains two default rules
- **Description**: List of rules for adjusting speaking frequency by conditions

Each `TalkRulesItem` contains the following fields:

| Field | Type | Description |
|------|------|------|
| `platform` | `str` | Platform identifier, empty means no platform restriction |
| `item_id` | `str` | User/Group ID, empty means no restriction |
| `rule_type` | `"group"` \| `"private"` | Chat flow type |
| `time` | `str` | Time period, format `"HH:MM-HH:MM"`, supports overnight |
| `value` | `float` | Speaking frequency value for this time period (0-1) |

**Default Rules**:

```toml
[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "00:00-08:59"
value = 0.8   # Slightly lower during early morning hours

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "09:00-18:59"
value = 1.0   # Normal during daytime
```

::: tip Time Period Format
Time periods support overnight, for example `"22:00-06:00"` means 10 PM to 6 AM the next day. When both `platform` and `item_id` are empty, it means global rules.
:::

## Message Receiving Configuration [message_receive]

Message receiving configuration controls how MaiBot processes received messages.

```toml
[message_receive]
image_parse_threshold = 5
ban_words = []
ban_msgs_regex = []
```

### image_parse_threshold

- **Type**: `int`
- **Default**: `5`
- **Description**: Image parsing threshold

When the number of images in a message does not exceed this threshold, MaiBot will parse the image content into text before processing. When the number of images exceeds this threshold, to avoid excessive parsing affecting performance, image parsing will be skipped.

For example, when set to 5:
- Message contains 1-5 images → Normal parsing
- Message contains 6+ images → Skip image parsing

### ban_words

- **Type**: `set[str]`
- **Default**: `set()`
- **Description**: Filter word list

Messages containing these words will be filtered, and MaiBot will not reply to these messages.

```toml
[message_receive]
ban_words = ["敏感词1", "敏感词2"]
```

### ban_msgs_regex

- **Type**: `set[str]`
- **Default**: `set()`
- **Description**: Filter regular expression list

Messages matching these regular expressions will be filtered. MaiBot will verify the validity of all regular expressions at startup.

```toml
[message_receive]
ban_msgs_regex = ["^\\[系统\\].*", "^抽奖.*"]
```

::: warning
Invalid regular expressions will cause MaiBot startup failure. Please ensure all regular expressions have correct syntax.
:::

## Complete Configuration Example

```toml
[chat]
talk_value = 0.8
mentioned_bot_reply = true
inevitable_at_reply = true
max_context_size = 30
planner_interrupt_max_consecutive_count = 2
enable_talk_value_rules = true

[chat.group_chat_prompt]
# Use multi-line string
value = """
你正在qq群里聊天，下面是群里正在聊的内容。
回复尽量简短一些。
"""

[chat.private_chat_prompts]
value = """
你正在聊天，下面是正在聊的内容。
回复尽量简短一些。
"""

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "00:00-08:59"
value = 0.6

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "09:00-23:59"
value = 1.0

[message_receive]
image_parse_threshold = 5
ban_words = []
ban_msgs_regex = []
```