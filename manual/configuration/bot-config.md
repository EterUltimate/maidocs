---
title: 基本配置
---

# 🤖 基本配置

`bot_config.toml` 中的 `[bot]` 段是机器人的"身份证"，告诉 MaiBot 一些最基本的信息。

## 📋 配置项一览

```toml
[bot]
platform = "qq"           # 平台标识
qq_account = 0            # QQ 账号
platforms = []            # 其他平台列表
nickname = "麦麦"          # 机器人昵称
alias_names = []          # 别名列表
```

## 🔍 每个配置项详解

### platform 🏷️ 平台标识

**这是什么？** 告诉 MaiBot 你在哪个聊天平台用她。

**怎么填？** 用 QQ 就填 `"qq"`，用其他平台就填对应的标识。

**举个例子：**
```toml
[bot]
platform = "qq"  # 我在QQ群里用麦麦
```

### qq_account 🔢 QQ账号

**这是什么？** 机器人的 QQ 号码，就像你的手机号一样。

**怎么填？** 填机器人登录的那个 QQ 号的数字 ID。

**举个例子：**
```toml
[bot]
qq_account = 123456789  # 麦麦的QQ号是123456789
```

::: tip 💡 小贴士
这个号码很重要！麦麦要靠它识别哪些消息是@了她。
:::

### platforms 📱 其他平台

**这是什么？** 如果麦麦要同时用在多个平台，就在这里列出其他平台。

**怎么填？** 填平台标识符的列表，比如 `["qq", "telegram"]`。

**举个例子：**
```toml
[bot]
platforms = ["telegram", "discord"]  # 除了QQ，还在Telegram和Discord用
```

::: warning ⚠️ 注意
大多数用户只需要用 QQ，这个可以留空。
:::

### nickname 🏷️ 机器人昵称

**这是什么？** 麦麦叫什么名字，大家在群里怎么称呼她。

**怎么填？** 想叫她什么就填什么，支持中文。

**举个例子：**
```toml
[bot]
nickname = "小麦"  # 大家叫她"小麦"
```

### alias_names 🏷️ 别名列表

**这是什么？** 麦麦的其他名字，就像你有大名还有小名一样。

**怎么填？** 填一个列表，可以有很多个名字。

**举个例子：**
```toml
[bot]
alias_names = ["麦麦", "小麦", "麦子"]  # 叫她哪个名字她都答应
```

::: tip 💡 小贴士
设置了别名后，群里有人用这些名字提到麦麦，她也会回复，就像被@了一样。
:::

## 🎯 完整配置示例

```toml
[bot]
platform = "qq"                    # 在QQ群里用
qq_account = 123456789             # 麦麦的QQ号
platforms = []                     # 只用QQ，其他平台留空
nickname = "麦麦"                   # 大名叫"麦麦"
alias_names = ["小麦", "麦子"]       # 小名叫"小麦"、"麦子"
```

---

## 🎭 人格、视觉配置

`bot_config.toml` 中的 `[personality]` 段和 `[visual]` 段是给麦麦"定人设"的地方。就像给虚拟角色写背景故事一样！

## 🎨 人格配置 [personality]

```toml
[personality]
personality = "是一个大二在读女大学生，现在正在上网和群友聊天，有时有点攻击性，有时比较温柔"
reply_style = "请不要刻意突出自身学科背景。可以参考贴吧，知乎和微博的回复风格。"
multiple_reply_style = []
multiple_probability = 0.3
```

### personality 🎭 "我是谁？"

**这是什么？** 麦麦的"人设"，就像小说角色的背景介绍。

**怎么写？** 用简单的话描述她是谁、什么性格。建议100字以内，越具体越好。

**举个例子：**
```toml
[personality]
personality = "是一个温柔耐心的女大学生，喜欢帮助别人，偶尔会开玩笑"

personality = "是一个毒舌但内心善良的大学生，嘴上不饶人但关键时刻靠得住"

personality = "是一个活泼开朗的高中女生，喜欢动漫和游戏，说话很有元气"
```

::: warning ⚠️ 注意
这个人设会直接影响麦麦的说话风格！写得太抽象或者前后矛盾，她说话就会怪怪的。
:::

### reply_style 💬 "我怎么说话？"

**这是什么？** 控制麦麦平时的说话风格，就像给她的语言习惯定规矩。

**怎么写？** 描述她应该怎么说话，用1-2行简单说明。

**举个例子：**
```toml
[personality]
reply_style = "请使用正式的中文表达，避免使用网络用语和表情符号。"

reply_style = "说话随意一些，常用颜文字和表情包，语气轻松。"

reply_style = "说话带点动漫感，偶尔用些日语词汇，语气活泼可爱。"
```

### multiple_reply_style 🎲 "我偶尔变个风格？"

**这是什么？** 给麦麦准备几个不同的说话风格，她会随机切换。

**怎么写？** 写一个风格列表，每个风格是一句话的描述。

**举个例子：**
```toml
[personality]
reply_style = "正常风格回复"
multiple_reply_style = [
    "用撒娇的语气回复",
    "用傲娇的语气回复", 
    "用吐槽的语气回复",
    "用文艺青年的语气回复"
]
multiple_probability = 0.3  # 30%概率换风格
```

### multiple_probability 🎲 "换风格的概率"

**这是什么？** 控制麦麦切换说话风格的频率。

**怎么填？** 0到1之间的小数，0表示永远不换，1表示每次都换。

**举个例子：**
```toml
[personality]
multiple_probability = 0.2  # 20%概率换风格（偶尔变一下）
multiple_probability = 0.8  # 80%概率换风格（经常变来变去）
```

## 👁️ 视觉配置 [visual]

控制麦麦怎么处理图片消息，能不能"看图说话"。

```toml
[visual]
planner_mode = "auto"
replyer_mode = "auto"
visual_style = "请用中文描述这张图片的内容。如果有文字，请把文字描述概括出来，请留意其主题，直观感受，输出为一段平文本，最多30字，请注意不要分点，就输出一段文本"
```

### planner_mode 🧠 "看到图后怎么规划？"

**这是什么？** 控制麦麦看到图片后，怎么决定要不要回复。

**选项说明：**
- `"auto"` - 自动检测（推荐）
- `"text"` - 当没看到图，纯文字处理
- `"multimodal"` - 强制看图理解（需要配置支持视觉的AI模型）

### replyer_mode 💬 "看到图后怎么回复？"

**这是什么？** 控制麦麦看到图片后，怎么生成回复内容。

**选项说明：**
- `"auto"` - 自动检测（推荐）
- `"text"` - 当没看到图，纯文字回复
- `"multimodal"` - 强制看图回复（需要配置支持视觉的AI模型）

::: tip 💡 小贴士
`"auto"`模式最聪明，会自动检查你配置的AI模型支不支持看图，然后决定怎么处理图片。
:::

### visual_style 👁️ "怎么看图说话？"

**这是什么？** 告诉AI怎么描述图片内容，就像给AI配副"眼镜"和使用说明。

**默认就很好！** 一般不建议修改，改坏了麦麦就看不懂图了。

## 🎯 完整配置示例

```toml
[personality]
personality = "是一个活泼开朗的高中女生，喜欢动漫和游戏，对新鲜事物很好奇，说话很有元气"
reply_style = "说话带点动漫感，偶尔用些日语词汇比如'哇'、'诶'，语气活泼可爱，喜欢用表情符号"
multiple_reply_style = [
    "用撒娇的语气回复，多用水灵灵的大眼睛表情",
    "用吐槽的语气回复，像看傻瓜一样的感觉",
    "用兴奋的语气回复，像发现了新大陆"
]
multiple_probability = 0.3

[visual]
planner_mode = "auto"    # 自动决定要不要看图
replyer_mode = "auto"    # 自动决定怎么回图
visual_style = "请用中文描述这张图片的内容。如果有文字，请把文字描述概括出来，请留意其主题，直观感受，输出为一段平文本，最多30字，请注意不要分点，就输出一段文本"
```

---

## 💬 聊天、消息接收配置

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
talk_value = 0.7                    # 中等活跃度
mentioned_bot_reply = true          # 提到名字就回复
inevitable_at_reply = true          # 被@必回复
max_context_size = 30               # 记住30条消息
enable_talk_value_rules = true      # 开启分时活跃

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

---

## 🧠 记忆配置

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
global_memory = false               # 只在当前群聊记忆（保护隐私）
enable_memory_query_tool = true     # 让麦麦主动回忆
memory_query_default_limit = 5      # 一次回忆5条

person_fact_writeback_enabled = true     # 记住人物信息
chat_summary_writeback_enabled = true    # 记住聊天摘要
chat_summary_writeback_message_threshold = 12  # 聊12条总结一次
chat_summary_writeback_context_length = 50     # 参考50条生成摘要

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

## ⚠️ 重要提醒

`bot_config.toml` 是 MaiBot 的核心配置文件，包含基础身份、人设、聊天行为、记忆、消息接收和视觉能力等设置。

**必须优先填写的两项：**
- ✅ `platform` - 告诉她在哪个平台
- ✅ `qq_account` - 告诉她的 QQ 号

**填完 bot 配置后，别忘了：**
1. 🤖 配置 [模型](./model-config.md) - 让麦麦有 AI 大脑
2. 🌐 配置 WebUI 或适配器 - 让麦麦能连接到你需要的平台

## 🚀 下一步

基础信息和行为配置好了，接下来：
- 🆕 **新手推荐**：去看 [模型配置](./model-config.md) 给麦麦装个 AI 大脑
- 🔌 **想连接平台**：去看 [适配器配置](../adapters/index.md)
