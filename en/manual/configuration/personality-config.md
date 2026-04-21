---
title: Personality Configuration
---

# 🎭 Defining Your Bot's Personality

The `[personality]` section and `[visual]` section in `bot_config.toml` are where you give MaiMai her "character setting". It's like writing a background story for a virtual character!

## 🎨 Personality Configuration [personality]

```toml
[personality]
personality = "是一个大二在读女大学生，现在正在上网和群友聊天，有时有点攻击性，有时比较温柔"
reply_style = "请不要刻意突出自身学科背景。可以参考贴吧，知乎和微博的回复风格。"
multiple_reply_style = []
multiple_probability = 0.3
```

### personality 🎭 "Who am I?"

**What is this?** MaiMai's "character setting", just like a novel character's background introduction.

**How to write?** Use simple words to describe who she is and what her personality is like. Recommended within 100 characters, the more specific the better.

**Examples:**
```toml
[personality]
# Gentle assistant type
personality = "A gentle and patient college girl who likes to help others and occasionally jokes"

# Sharp-tongued but kind type  
personality = "A sharp-tongued but kind-hearted college student who doesn't hold back verbally but is reliable when it counts"

# Lively and cute type
personality = "An energetic high school girl who likes anime and games, speaks with lots of vitality"
```

::: warning ⚠️ Note
This character setting will directly affect MaiMai's speaking style! If written too abstractly or inconsistently, her speech will feel weird.
:::

### reply_style 💬 "How do I speak?"

**What is this?** Controls MaiMai's usual speaking style, like setting rules for her language habits.

**How to write?** Describe how she should speak, with 1-2 lines of simple instructions.

**Examples:**
```toml
[personality]
# More formal
reply_style = "Please use formal Chinese expression, avoid internet slang and emoji symbols."

# More casual
reply_style = "Speak casually, often use kaomoji and emojis, relaxed tone."

# Anime style
reply_style = "Speak with some anime feeling, occasionally use Japanese words like 'wow', 'eh', lively and cute tone."
```

### multiple_reply_style 🎲 "Occasionally change style?"

**What is this?** Prepare several different speaking styles for MaiMai, she'll randomly switch between them.

**How to write?** Write a list of styles, each style is a one-sentence description.

**Example:**
```toml
[personality]
reply_style = "Normal style reply"
multiple_reply_style = [
    "Reply with a coquettish tone",
    "Reply with a tsundere tone", 
    "Reply with a complaining tone",
    "Reply with an artsy youth tone"
]
multiple_probability = 0.3  # 30% chance to change style
```

### multiple_probability 🎲 "Probability of changing style"

**What is this?** Controls how frequently MaiMai switches speaking styles.

**How to fill?** A decimal between 0 and 1, 0 means never change, 1 means change every time.

**Examples:**
```toml
[personality]
multiple_probability = 0.2  # 20% chance to change style (occasionally changes)
multiple_probability = 0.8  # 80% chance to change style (changes frequently)
```

## 👁️ Visual Configuration [visual]

Controls how MaiMai processes image messages and whether she can "talk about pictures".

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
- `"text"` - Treat as if no image, pure text processing
- `"multimodal"` - Force image understanding (requires AI model that supports vision)

### replyer_mode 💬 "How to reply after seeing an image?"

**What is this?** Controls how MaiMai generates reply content after seeing an image.

**Options:**
- `"auto"` - Automatic detection (recommended)
- `"text"` - Treat as if no image, pure text reply
- `"multimodal"` - Force image reply (requires AI model that supports vision)

::: tip 💡 Quick Tip
`"auto"` mode is the smartest, it will automatically check if your configured AI model supports image viewing, then decide how to process images.
:::

### visual_style 👁️ "How to talk about pictures?"

**What is this?** Tell the AI how to describe image content, like giving AI a pair of "glasses" and usage instructions.

**Default is great!** Generally not recommended to modify, if broken MaiMai won't understand images.

## 🎯 Complete Configuration Example

```toml
[personality]
# Lively high school girl character setting
personality = "An energetic high school girl who likes anime and games, curious about new things, speaks with lots of vitality"
reply_style = "Speak with some anime feeling, occasionally use Japanese words like 'wow', 'eh', lively and cute tone, likes to use emoji symbols"
multiple_reply_style = [
    "Reply with coquettish tone, use lots of big watery eyes expressions",
    "Reply with complaining tone, like looking at a fool",
    "Reply with excited tone, like discovering a new continent"
]
multiple_probability = 0.3

[visual]
planner_mode = "auto"    # Automatically decide whether to look at images
replyer_mode = "auto"    # Automatically decide how to reply to images
visual_style = "请用中文描述这张图片的内容。如果有文字，请把文字描述概括出来，请留意其主题，直观感受，输出为一段平文本，最多30字，请注意不要分点，就输出一段文本"
```

## 🚀 What to Read Next?

- 💬 **Want to adjust chat frequency**: See [Chat Config](./chat-config.md)
- 🧠 **Want to enable memory**: See [Memory Config](./memory-config.md)
- 🤖 **Want to change AI brain**: See [Model Config](./model-config.md)