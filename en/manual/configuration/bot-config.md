---
title: Basic Configuration
---

# 🤖 Basic Configuration

The `[bot]` section in `bot_config.toml` is the bot's "ID card". It tells MaiBot the most basic information.

## 📋 Configuration Items Overview

```toml
[bot]
platform = "qq"           # Platform identifier
qq_account = 0            # QQ account
platforms = []            # Other platform list
nickname = "MaiMai"       # Bot nickname
alias_names = []          # Alias list
```

## 🔍 Detailed Explanation of Each Configuration Item

### platform 🏷️ Platform Identifier

**What is this?** Tells MaiBot which chat platform you use her on.

**How to fill?** Use `"qq"` for QQ. For other platforms, fill in the corresponding platform identifier.

**Example:**
```toml
[bot]
platform = "qq"  # I use MaiMai in QQ groups
```

### qq_account 🔢 QQ Account

**What is this?** The bot's QQ number, just like a phone number.

**How to fill?** Fill in the numeric ID of the QQ account that the bot logs in with.

**Example:**
```toml
[bot]
qq_account = 123456789  # MaiMai's QQ number is 123456789
```

::: tip 💡 Tip
This number is important! MaiMai uses it to identify which messages are @mentions to her.
:::

### platforms 📱 Other Platforms

**What is this?** If MaiMai needs to run on multiple platforms at the same time, list the other platforms here.

**How to fill?** Fill in a list of platform identifiers, such as `["qq", "telegram"]`.

**Example:**
```toml
[bot]
platforms = ["telegram", "discord"]  # Besides QQ, also use Telegram and Discord
```

::: warning ⚠️ Note
Most users only need QQ, so this can be left empty.
:::

### nickname 🏷️ Bot Nickname

**What is this?** What MaiMai is called and how people address her in groups.

**How to fill?** Use whatever name you want. Chinese is supported.

**Example:**
```toml
[bot]
nickname = "XiaoMai"  # Everyone calls her "XiaoMai"
```

### alias_names 🏷️ Alias List

**What is this?** MaiMai's other names, like having both a formal name and nicknames.

**How to fill?** Fill in a list. You can set many names.

**Example:**
```toml
[bot]
alias_names = ["MaiMai", "XiaoMai", "MaiZi"]  # She responds to any of these names
```

::: tip 💡 Tip
After aliases are configured, when someone mentions MaiMai by these names in a group, she will also reply, just like being @mentioned.
:::

## 🎯 Complete Configuration Example

```toml
[bot]
platform = "qq"                    # Use in QQ groups
qq_account = 123456789             # MaiMai's QQ number
platforms = []                     # Only use QQ, leave other platforms empty
nickname = "MaiMai"                # Main name is "MaiMai"
alias_names = ["XiaoMai", "MaiZi"] # Nicknames are "XiaoMai" and "MaiZi"
```

---

## 🎭 Personality and Visual Configuration

The `[personality]` section and `[visual]` section in `bot_config.toml` are where you define MaiMai's character setting. It is like writing a background story for a virtual character.

## 🎨 Personality Configuration [personality]

```toml
[personality]
personality = "是一个大二在读女大学生，现在正在上网和群友聊天，有时有点攻击性，有时比较温柔"
reply_style = "请不要刻意突出自身学科背景。可以参考贴吧，知乎和微博的回复风格。"
multiple_reply_style = []
multiple_probability = 0.3
```

### personality 🎭 "Who am I?"

**What is this?** MaiMai's character setting, like a character background in a novel.

**How to write?** Describe who she is and what kind of personality she has in simple words. Keep it within about 100 characters if possible. The more specific, the better.

**Examples:**
```toml
[personality]
personality = "A gentle and patient college girl who likes helping others and occasionally jokes"

personality = "A sharp-tongued but kind-hearted college student who talks tough but is reliable when it matters"

personality = "A lively high school girl who likes anime and games and speaks with lots of energy"
```

::: warning ⚠️ Note
This character setting directly affects MaiMai's speaking style. If it is too vague or self-contradictory, her replies may feel strange.
:::

### reply_style 💬 "How do I speak?"

**What is this?** Controls MaiMai's usual speaking style, like setting rules for her language habits.

**How to write?** Describe how she should speak in one or two short lines.

**Examples:**
```toml
[personality]
reply_style = "Please use formal Chinese expression and avoid internet slang and emojis."

reply_style = "Speak more casually, often use kaomoji and stickers, and keep a relaxed tone."

reply_style = "Speak with a little anime flavor, occasionally use Japanese words, and keep the tone lively and cute."
```

### multiple_reply_style 🎲 "Occasionally change style?"

**What is this?** Prepare several different speaking styles for MaiMai. She will randomly switch between them.

**How to write?** Write a list of styles. Each style is a one-sentence description.

**Example:**
```toml
[personality]
reply_style = "Reply in the normal style"
multiple_reply_style = [
    "Reply in a coquettish tone",
    "Reply in a tsundere tone", 
    "Reply in a teasing tone",
    "Reply in an artsy youth tone"
]
multiple_probability = 0.3  # 30% chance to change style
```

### multiple_probability 🎲 "Probability of changing style"

**What is this?** Controls how often MaiMai switches speaking styles.

**How to fill?** A decimal between 0 and 1. `0` means never switch, and `1` means switch every time.

**Examples:**
```toml
[personality]
multiple_probability = 0.2  # 20% chance to change style, occasionally
multiple_probability = 0.8  # 80% chance to change style, very often
```

## 👁️ Visual Configuration [visual]

Controls how MaiMai handles image messages and whether she can "talk about pictures".

```toml
[visual]
planner_mode = "auto"
replyer_mode = "auto"
visual_style = "请用中文描述这张图片的内容。如果有文字，请把文字描述概括出来，请留意其主题，直观感受，输出为一段平文本，最多30字，请注意不要分点，就输出一段文本"
```

### planner_mode 🧠 "How to plan after seeing an image?"

**What is this?** Controls how MaiMai decides whether to reply after seeing an image.

**Options:**
- `"auto"` - Automatic detection (recommended)
- `"text"` - Treat as text-only processing, as if no image was seen
- `"multimodal"` - Force image understanding (requires a vision-capable AI model)

### replyer_mode 💬 "How to reply after seeing an image?"

**What is this?** Controls how MaiMai generates reply content after seeing an image.

**Options:**
- `"auto"` - Automatic detection (recommended)
- `"text"` - Treat as text-only reply, as if no image was seen
- `"multimodal"` - Force image-based reply (requires a vision-capable AI model)

::: tip 💡 Tip
`"auto"` mode is the smartest. It automatically checks whether your configured AI model supports images, then decides how to process them.
:::

### visual_style 👁️ "How to talk about pictures?"

**What is this?** Tells the AI how to describe image content, like giving the AI a pair of "glasses" and instructions.

**The default is good enough.** Usually you do not need to change it. If changed poorly, MaiMai may not understand images well.

## 🎯 Complete Configuration Example

```toml
[personality]
personality = "A lively high school girl who likes anime and games, is curious about new things, and speaks with lots of energy"
reply_style = "Speak with a little anime flavor, occasionally use words like 'wow' and 'eh', keep the tone lively and cute, and like using emojis"
multiple_reply_style = [
    "Reply in a coquettish tone and use more sparkling-eye expressions",
    "Reply in a teasing tone, like looking at a fool",
    "Reply in an excited tone, like discovering a new continent"
]
multiple_probability = 0.3

[visual]
planner_mode = "auto"    # Automatically decide whether to look at images
replyer_mode = "auto"    # Automatically decide how to reply to images
visual_style = "请用中文描述这张图片的内容。如果有文字，请把文字描述概括出来，请留意其主题，直观感受，输出为一段平文本，最多30字，请注意不要分点，就输出一段文本"
```

---

## 💬 Chat and Message Receiving Configuration

The `[chat]` section and `[message_receive]` section in `bot_config.toml` control MaiMai's chat behavior. They are like teaching her "when to speak and when to stay quiet".

## 🎛️ Chat Configuration [chat]

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

### talk_value 🎚️ "How talkative is MaiMai?"

**What is this?** Controls MaiMai's talkativeness. `0` is the most silent, `1` is the most talkative.

**How to fill?** A decimal between 0 and 1, such as `0.3`, `0.7`, or `1.0`.

**Effect comparison:**
| Value | Effect | Analogy |
|------|------|------|
| `0.0` | Almost never speaks | Cool honor student |
| `0.5` | Occasionally says something | Normal classmate |
| `1.0` | Speaks actively | Talkative friend |

**Examples:**
```toml
[chat]
talk_value = 0.3  # MaiMai is quieter and occasionally joins in
talk_value = 0.8  # MaiMai is more active and often joins discussions
```

### mentioned_bot_reply 🏷️ "Reply when her name is mentioned?"

**What is this?** Controls whether MaiMai replies when someone mentions her name in a group, but does not @ her.

**How to fill?** `true` means reply, `false` means ignore it.

**Examples:**
```toml
[chat]
mentioned_bot_reply = true   # If someone says "where is MaiMai?", she appears
mentioned_bot_reply = false  # She only replies when @mentioned
```

### inevitable_at_reply 📧 "Always reply when @mentioned?"

**What is this?** Controls whether MaiMai must reply when she is @mentioned, regardless of `talk_value`.

**How to fill?** `true` means always reply, `false` means reply depending on mood.

**Examples:**
```toml
[chat]
inevitable_at_reply = true   # Always reply when @mentioned (recommended)
inevitable_at_reply = false  # May ignore @mentions, which can feel aloof
```

### max_context_size 🧠 "How many messages can she remember?"

**What is this?** Controls how many recent chat messages MaiMai can "remember" at once to understand context.

**How to fill?** A number. Larger values preserve more context, but also consume more AI resources.

**Examples:**
```toml
[chat]
max_context_size = 20  # Remember the latest 20 messages
max_context_size = 50  # Remember the latest 50 messages, better context
```

::: tip 💡 Tip
Usually 30 messages is enough. Too many can make the conversation more confusing.
:::

### enable_talk_value_rules ⏰ "Adjust talkativeness by time?"

**What is this?** Lets MaiMai use different activity levels in different time periods.

**How to fill?** `true` to enable, `false` to disable.

**Examples:**
```toml
[chat]
enable_talk_value_rules = true   # Enable time-based activity (recommended)
enable_talk_value_rules = false  # Use the same activity level all day
```

### talk_value_rules 📅 "Specific time period rules"

**What is this?** Sets different activity levels for different time periods.

**How to write?** Write multiple rules. Each rule includes a time period and activity value.

**Example:**
```toml
[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "00:00-08:59"  # Midnight to 9 AM
value = 0.3           # MaiMai is quiet and barely speaks

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "09:00-18:59"  # Daytime work hours
time = "09:00-23:59"  # 9 AM to 11 PM
value = 0.8           # MaiMai is more active and often joins discussions

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "22:00-23:59"  # After 10 PM
value = 0.5           # MaiMai is quieter and occasionally says something
```

::: tip ⏰ Time Format
Overnight periods are supported. For example, `"22:00-06:00"` means 10 PM to 6 AM the next day.
:::

## 📨 Message Receiving Configuration [message_receive]

Controls how MaiMai handles received messages, including which messages should be processed and which should be ignored.

```toml
[message_receive]
image_parse_threshold = 5
ban_words = []
ban_msgs_regex = []
```

### image_parse_threshold 🖼️ "How many images to parse at once?"

**What is this?** Controls the maximum number of images parsed in a single message, avoiding performance issues caused by too many images.

**How to fill?** A number. If the number of images exceeds it, MaiMai will not parse them.

**Examples:**
```toml
[message_receive]
image_parse_threshold = 3  # Ignore image parsing when there are more than 3 images
image_parse_threshold = 10 # Can parse many images, but may be slower
```

### ban_words 🚫 "Words I do not like"

**What is this?** Sets sensitive words. Messages containing these words will be ignored by MaiMai.

**How to fill?** Write a list of words.

**Example:**
```toml
[message_receive]
ban_words = ["advertisement", "promotion", "scam"]  # Ignore messages containing these words
```

### ban_msgs_regex 🚫 "Message formats I ignore"

**What is this?** Uses regular expressions to match message formats you do not want MaiMai to handle.

**How to fill?** Write a list of regular expressions. You need to understand regex syntax.

**Example:**
```toml
[message_receive]
ban_msgs_regex = [
    "^\\[System\\].*",    # Ignore system messages
    "^Lottery.*",         # Ignore lottery messages
    ".*join group.*"      # Ignore group-joining related messages
]
```

::: warning ⚠️ Note
Invalid regular expressions can cause MaiMai to fail to start. If you do not know how to use regex, leave this empty.
:::

## 🎯 Complete Configuration Example

```toml
[chat]
talk_value = 0.7                    # Medium activity
mentioned_bot_reply = true          # Reply when name is mentioned
inevitable_at_reply = true          # Always reply when @mentioned
max_context_size = 30               # Remember 30 messages
enable_talk_value_rules = true      # Enable time-based activity

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "00:00-08:59"
value = 0.2   # Very quiet in early morning

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "09:00-22:59"
value = 0.8   # Very active during the day

[[chat.talk_value_rules]]
platform = ""
item_id = ""
rule_type = "group"
time = "23:00-23:59"
value = 0.3   # Quieter at night

[message_receive]
image_parse_threshold = 5           # Parse at most 5 images
ban_words = ["advertisement", "promotion"] # Ignore ads
ban_msgs_regex = []                 # No regex filtering
```

---

## 🧠 Memory Configuration

The `[memory]` section in `bot_config.toml` gives MaiMai "memory", so she can remember your conversations like a person and understand you better over time.

## 🧠 What is the memory system?

Imagine MaiMai has a small notebook that records:
- ✅ Your preferences and habits ("Xiao Ming likes cats and dislikes dogs")
- ✅ Important things you talked about ("Last week you said you wanted to travel")
- ✅ Various interesting information ("Xiao Hong is a programmer and can play guitar")

The more she remembers, the better she understands you, and the more natural the chat becomes.

## 📋 Basic Memory Configuration

```toml
[memory]
global_memory = false
enable_memory_query_tool = true
memory_query_default_limit = 5
person_fact_writeback_enabled = true
chat_summary_writeback_enabled = true
chat_summary_writeback_message_threshold = 12
chat_summary_writeback_context_length = 50
```

### global_memory 🌍 "Can memories cross chats?"

**What is this?** Controls whether MaiMai can remember content from one group while chatting in another group.

**How to fill?** `true` allows cross-chat memory, `false` only remembers within the current chat.

**Examples:**
```toml
[memory]
global_memory = false  # Conservative setting, only remember current chat (recommended)
global_memory = true   # Open setting, all chats share memory
```

::: warning ⚠️ Privacy Reminder
Enabling global memory may involve privacy issues. It is recommended to use it together with a blacklist.
:::

### enable_memory_query_tool 🔍 "Let MaiMai actively search memory?"

**What is this?** Lets MaiMai actively "flip through her notebook" during chat to look up previous records.

**How to fill?** `true` to enable, `false` to disable.

**Examples:**
```toml
[memory]
enable_memory_query_tool = true   # MaiMai actively recalls things (recommended)
enable_memory_query_tool = false  # MaiMai does not actively query memory
```

### memory_query_default_limit 🔢 "How many memories to query at once?"

**What is this?** Controls how many things MaiMai can recall at most each time.

**How to fill?** A number between 1 and 20.

**Examples:**
```toml
[memory]
memory_query_default_limit = 3   # Recall 3 related memories
memory_query_default_limit = 8   # Recall 8 related memories
```

## 📝 Automatic Memory Recording

MaiMai has two "assistants" that automatically help her take notes:

### person_fact_writeback_enabled 👤 "Remember person information"

**What is this?** After each reply, MaiMai automatically extracts information about people and records it.

**How to fill?** `true` to enable, `false` to disable.

**What does it remember?** Personal information such as preferences, identity, and habits.

**Examples:**
```toml
[memory]
person_fact_writeback_enabled = true   # MaiMai remembers what kind of person you are (recommended)
person_fact_writeback_enabled = false  # MaiMai does not remember person information
```

### chat_summary_writeback_enabled 💬 "Remember chat summaries"

**What is this?** After chatting for a while, MaiMai automatically summarizes the conversation and records it.

**How to fill?** `true` to enable, `false` to disable.

**What does it remember?** Main topics, important events, and interesting content from the chat.

**Examples:**
```toml
[memory]
chat_summary_writeback_enabled = true   # MaiMai summarizes chat content (recommended)
chat_summary_writeback_enabled = false  # MaiMai does not remember chat summaries
```

### chat_summary_writeback_message_threshold 📊 "How many messages before summarizing?"

**What is this?** Controls how many messages must accumulate before an automatic summary is generated.

**How to fill?** A number. Larger values summarize less frequently.

**Examples:**
```toml
[memory]
chat_summary_writeback_message_threshold = 8   # Summarize after 8 messages, frequent
chat_summary_writeback_message_threshold = 20  # Summarize after 20 messages, saves resources
```

### chat_summary_writeback_context_length 📖 "How many messages to read when summarizing?"

**What is this?** Controls how many chat messages are referenced when generating a summary.

**How to fill?** A number. Larger values produce more detailed summaries, but consume more AI resources.

**Examples:**
```toml
[memory]
chat_summary_writeback_context_length = 30  # Use 30 messages to generate summary
chat_summary_writeback_context_length = 80  # Use 80 messages to generate summary, more detailed
```

## 🔧 Advanced Features (Beginners Can Skip)

### Feedback correction system (disabled by default)

MaiMai checks later user feedback. If she finds that she remembered something incorrectly before, she can automatically correct it.

```toml
[memory]
feedback_correction_enabled = false  # Disabled by default, recommended for beginners
```

::: tip 💡 Recommendation
This feature is relatively complex. Consider enabling it after you are familiar with the basic functions.
:::

## 🎯 Recommended Configuration (For Beginners)

```toml
[memory]
global_memory = false               # Only remember current chat (protect privacy)
enable_memory_query_tool = true     # Let MaiMai actively recall
memory_query_default_limit = 5      # Recall 5 memories at once

person_fact_writeback_enabled = true     # Remember person information
chat_summary_writeback_enabled = true    # Remember chat summaries
chat_summary_writeback_message_threshold = 12  # Summarize after 12 messages
chat_summary_writeback_context_length = 50     # Use 50 messages for summary

feedback_correction_enabled = false   # Do not enable correction yet
```

## 🚀 Memory System Workflow

```
You chat → MaiMai replies
     ↓
Assistant 1: "This person seems to like cats" → Write to notebook
     ↓
Assistant 2: "There have been many messages, summarize them" → Write to notebook
     ↓
Next chat → MaiMai checks notebook → "Right, you like cats!"
```

## ⚠️ Important Reminder

`bot_config.toml` is MaiBot's core configuration file. It contains basic identity, character setting, chat behavior, memory, message receiving, and visual capability settings.

**Two items that should be configured first:**
- ✅ `platform` - Tell MaiBot which platform to use
- ✅ `qq_account` - Tell MaiBot her QQ number

**After finishing bot configuration, don't forget:**
1. 🤖 Configure [Model](./model-config.md) - Give MaiMai an AI brain
2. 🌐 Configure WebUI or adapters - Let MaiMai connect to the platform you need

## 🚀 Next Steps

After basic information and behavior are configured:
- 🆕 **Newbie Recommended**: Go see [Model Configuration](./model-config.md) to give MaiMai an AI brain
- 🔌 **Want to connect a platform**: See [Adapter Configuration](../adapters/index.md)
