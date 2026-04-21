---
title: 记忆配置
---

# 🧠 机器人怎么记住你

`bot_config.toml` 中的 `[memory]` 段让麦麦拥有"记忆力"，就像人一样能记住你们的聊天内容，越用越懂你！

## 🧠 什么是记忆系统？

想象麦麦有个小本本，会记录：
- ✅ 你的喜好和习惯（"小明喜欢猫，讨厌狗"）
- ✅ 你们聊过的重要事情（"上周说要去旅游"）
- ✅ 各种有趣的信息（"小红是程序员，会弹吉他"）

记得越多，麦麦就越懂你，聊天就越自然！

## 📋 基础记忆配置

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

### global_memory 🌍 "能不能跨群聊记东西？"

**这是什么？** 控制麦麦能不能在一个群里记住另一个群聊过的内容。

**怎么填？** `true` 可以跨群记，`false` 只在当前群记。

**举个例子：**
```toml
[memory]
global_memory = false  # 保守设置，只在当前群聊记忆（推荐）
global_memory = true   # 开放设置，所有群聊共享记忆
```

::: warning ⚠️ 隐私提醒
开启全局记忆可能涉及隐私问题！建议配合黑名单使用。
:::

### enable_memory_query_tool 🔍 "让麦麦主动查记忆？"

**这是什么？** 让麦麦在聊天时能主动"翻小本本"查之前的记录。

**怎么填？** `true` 开启，`false` 关闭。

**举个例子：**
```toml
[memory]
enable_memory_query_tool = true   # 麦麦会主动回忆（推荐）
enable_memory_query_tool = false  # 麦麦不主动查记忆
```

### memory_query_default_limit 🔢 "一次查多少条记忆？"

**这是什么？** 控制麦麦一次最多回忆多少件事情。

**怎么填？** 数字，1到20之间。

**举个例子：**
```toml
[memory]
memory_query_default_limit = 3   # 回忆3条相关记忆
memory_query_default_limit = 8   # 回忆8条相关记忆
```

## 📝 记忆自动记录

麦麦有两个"小秘书"，会自动帮她记笔记：

### person_fact_writeback_enabled 👤 "记住人物信息"

**这是什么？** 麦麦每次回复后，自动提取关于人的信息记下来。

**怎么填？** `true` 开启，`false` 关闭。

**会记什么？** 你的喜好、身份、习惯等个人信息。

**举个例子：**
```toml
[memory]
person_fact_writeback_enabled = true   # 麦麦会记住你是怎样的人（推荐）
person_fact_writeback_enabled = false  # 麦麦不记人物信息
```

### chat_summary_writeback_enabled 💬 "记住聊天摘要"

**这是什么？** 聊一段时间后，自动总结聊天内容记下来。

**怎么填？** `true` 开启，`false` 关闭。

**会记什么？** 聊天的主要话题、重要事件、有趣内容。

**举个例子：**
```toml
[memory]
chat_summary_writeback_enabled = true   # 麦麦会总结聊天内容（推荐）
chat_summary_writeback_enabled = false  # 麦麦不记聊天摘要
```

### chat_summary_writeback_message_threshold 📊 "聊多少条才总结？"

**这是什么？** 控制积累多少条消息后才自动总结一次。

**怎么填？** 数字，越大总结越不频繁。

**举个例子：**
```toml
[memory]
chat_summary_writeback_message_threshold = 8   # 聊8条就总结（频繁）
chat_summary_writeback_message_threshold = 20  # 聊20条才总结（节省）
```

### chat_summary_writeback_context_length 📖 "总结时看多少条？"

**这是什么？** 控制生成摘要时参考多少条聊天记录。

**怎么填？** 数字，越大摘要越详细，但也越费AI算力。

**举个例子：**
```toml
[memory]
chat_summary_writeback_context_length = 30  # 参考30条消息生成摘要
chat_summary_writeback_context_length = 80  # 参考80条消息生成摘要（详细）
```

## 🔧 高级功能（新手可跳过）

### 反馈纠错系统（默认关闭）

麦麦会检查用户后续的反馈，如果发现之前记错了，会自动纠正。

```toml
[memory]
feedback_correction_enabled = false  # 默认关闭，新手建议保持关闭
```

::: tip 💡 建议
这个功能比较复杂，建议熟悉基础功能后再考虑开启。
:::

## 🎯 推荐配置（新手专用）

```toml
[memory]
# 基础记忆功能（推荐新手使用）
global_memory = false               # 只在当前群聊记忆（保护隐私）
enable_memory_query_tool = true     # 让麦麦主动回忆
memory_query_default_limit = 5      # 一次回忆5条

# 自动记录功能
person_fact_writeback_enabled = true     # 记住人物信息
chat_summary_writeback_enabled = true    # 记住聊天摘要
chat_summary_writeback_message_threshold = 12  # 聊12条总结一次
chat_summary_writeback_context_length = 50     # 参考50条生成摘要

# 高级功能（新手保持关闭）
feedback_correction_enabled = false   # 纠错功能先不开
```

## 🚀 记忆系统工作流程

```
你们聊天 → 麦麦回复
     ↓
小秘书1："这个人好像喜欢猫" → 记到小本本
     ↓
小秘书2："聊了好多条了，总结一下" → 记到小本本
     ↓
下次聊天 → 麦麦查小本本 → "对了，你喜欢猫！"
```

## 🚀 下一步看什么？

- 🤖 **想换AI大脑**：看 [模型配置](./model-config.md)
- 🎭 **想调性格**：看 [人格配置](./personality-config.md)
- 💬 **想调聊天频率**：看 [聊天配置](./chat-config.md)