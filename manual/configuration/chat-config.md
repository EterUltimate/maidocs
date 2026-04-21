---
title: 聊天配置
---

# 💬 机器人怎么聊天

`bot_config.toml` 中的 `[chat]` 段和 `[message_receive]` 段控制麦麦的聊天行为，就像教她"什么时候该说话，什么时候该闭嘴"。

## 🎛️ 聊天配置 [chat]

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
```

### talk_value 🎚️ "麦麦话多不多？"

**这是什么？** 控制麦麦的**话痨程度**，0最沉默，1最话痨。

**怎么填？** 0到1之间的小数，比如0.3、0.7、1.0。

**效果对比：**
| 数值 | 效果 | 类比 |
|------|------|------|
| `0.0` | 几乎不说话 | 高冷学霸 |
| `0.5` | 偶尔说几句 | 正常同学 |
| `1.0` | 积极发言 | 话痨朋友 |

**举个例子：**
```toml
[chat]
talk_value = 0.3  # 麦麦比较安静，偶尔插话
talk_value = 0.8  # 麦麦比较活跃，经常参与讨论
```

### mentioned_bot_reply 🏷️ "提到名字就回复？"

**这是什么？** 控制群里有人提到麦麦的名字（不是@）时，她要不要回复。

**怎么填？** `true` 表示回复，`false` 表示不理。

**举个例子：**
```toml
[chat]
mentioned_bot_reply = true   # 有人说"麦麦呢？"她就会出来
mentioned_bot_reply = false  # 只有@她才会回复
```

### inevitable_at_reply 📧 "被@了必回复？"

**这是什么？** 控制麦麦被@时是否**必须回复**，不管talk_value设置多少。

**怎么填？** `true` 表示必回复，`false` 表示看心情。

**举个例子：**
```toml
[chat]
inevitable_at_reply = true   # 被@了一定回复（推荐）
inevitable_at_reply = false  # 被@了也可能不回（可能显得高冷）
```

### max_context_size 🧠 "能记住多少条消息？"

**这是什么？** 控制麦麦一次能"记住"多少条聊天记录，用来理解上下文。

**怎么填？** 数字，越大记住的越多，但也越费AI算力。

**举个例子：**
```toml
[chat]
max_context_size = 20  # 记住最近20条消息
max_context_size = 50  # 记住最近50条消息（更懂上下文）
```

::: tip 💡 小贴士
一般30条就够用了，太多了反而容易混乱。
:::

### enable_talk_value_rules ⏰ "按时间调整话多话少？"

**这是什么？** 让麦麦在不同时间段有不同的活跃程度。

**怎么填？** `true` 开启，`false` 关闭。

**举个例子：**
```toml
[chat]
enable_talk_value_rules = true   # 开启分时活跃（推荐）
enable_talk_value_rules = false  # 全天一个活跃度
```

### talk_value_rules 📅 "具体的时间段规则"

**这是什么？** 给不同时间段设置不同的活跃程度。

**怎么写？** 写多个规则，每个规则包含时间段和活跃值。

**举个例子：**
```toml
[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "00:00-08:59"  # 凌晨到早上9点
value = 0.3           # 麦麦很安静，基本不说话

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "09:00-18:59"  # 白天工作时间
time = "09:00-23:59"  # 早上9点到晚上11点
value = 0.8           # 麦麦比较活跃，经常参与讨论

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "22:00-23:59"  # 晚上10点后
value = 0.5           # 麦麦比较安静，偶尔说几句
```

::: tip ⏰ 时间格式
支持跨夜，比如 `"22:00-06:00"` 表示晚上10点到第二天早上6点。
:::

## 📨 消息接收配置 [message_receive]

控制麦麦怎么处理收到的消息，哪些该理，哪些不该理。

```toml
[message_receive]
image_parse_threshold = 5
ban_words = []
ban_msgs_regex = []
```

### image_parse_threshold 🖼️ "一次看几张图？"

**这是什么？** 控制一次消息里最多解析几张图片，避免太多图影响性能。

**怎么填？** 数字，超过这个数量的图就不看了。

**举个例子：**
```toml
[message_receive]
image_parse_threshold = 3  # 超过3张图就不看了
image_parse_threshold = 10 # 可以看很多图（但可能变慢）
```

### ban_words 🚫 "这些词我不爱听"

**这是什么？** 设置一些敏感词，包含这些词的消息麦麦会忽略。

**怎么填？** 写一个词语列表。

**举个例子：**
```toml
[message_receive]
ban_words = ["广告", "推销", "诈骗"]  # 看到这些词就不理
```

### ban_msgs_regex 🚫 "这些格式的消息我不理"

**这是什么？** 用正则表达式匹配不想理的消息格式。

**怎么填？** 写正则表达式列表，需要懂正则语法。

**举个例子：**
```toml
[message_receive]
ban_msgs_regex = [
    "^\\[系统\\].*",    # 不理系统消息
    "^抽奖.*",         # 不理抽奖消息
    ".*加群.*"         # 不理加群相关消息
]
```

::: warning ⚠️ 注意
正则表达式写错了会导致麦麦启动失败！不会用就别写。
:::

## 🎯 完整配置示例

```toml
[chat]
# 麦麦的聊天习惯
talk_value = 0.7                    # 中等活跃度
mentioned_bot_reply = true          # 提到名字就回复
inevitable_at_reply = true          # 被@必回复
max_context_size = 30               # 记住30条消息
enable_talk_value_rules = true      # 开启分时活跃

# 时间段规则：白天活跃，晚上安静
[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "00:00-08:59"
value = 0.2   # 凌晨很安静

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "09:00-22:59"
value = 0.8   # 白天很活跃

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "23:00-23:59"
value = 0.3   # 晚上较安静

[message_receive]
image_parse_threshold = 5           # 最多看5张图
ban_words = ["广告", "推销"]         # 不理广告
ban_msgs_regex = []                 # 不用正则过滤
```

## 🚀 下一步看什么？

- 🧠 **想让麦麦记住你**：看 [记忆配置](./memory-config.md)
- 🤖 **想换AI大脑**：看 [模型配置](./model-config.md)
- 🎭 **想调性格**：看 [人格配置](./personality-config.md)