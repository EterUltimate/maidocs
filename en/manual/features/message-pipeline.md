---
title: How Messages Are Processed
---

# How Messages Are Processed 📨

Have you ever wondered how MaiBot handles your messages when you @ it or chat with it? Let us explain this process in simple terms.

## One Sentence Version

**You send message → Bot thinks → Bot replies**

That's it! But there are actually many interesting little steps in between.

## Detailed Process (In Plain Language)

### 1️⃣ Receiving the Message

When you send a message in the group:
- If you @ the bot, it will definitely see it
- If you don't @ it, it will still secretly check and think about whether to join in
- Whether it's text, images, or emojis, it can receive them all

### 2️⃣ First Check: Should I Respond?

The bot will quickly judge:
- Is this spam? (Filter out if there are blocked words)
- Is this a command? (Like "!help" etc.)
- If it's a command, handle the command first

### 3️⃣ Enter Thinking Mode

If it's not a command, the bot starts thinking seriously:
- Look at what you were talking about before
- Recall your personal characteristics
- Think about whether now is a good time to join the conversation

### 4️⃣ Decide Whether to Reply

This step is crucial! The bot will consider:
- What's the current chat atmosphere like?
- Will speaking up interrupt you?
- What's appropriate to say?

Sometimes it will choose:
- **Reply immediately** - Feels it's time to speak
- **Wait and see** - Feels the timing isn't right
- **Stay silent** - Decides not to join in

### 5️⃣ Organize Language

If it decides to reply, the bot will:
- Think about what tone to use
- Recall your group's speaking style
- Choose appropriate emojis (if needed)
- Organize a natural response

### 6️⃣ Send Reply

Finally, it sends out the well-thought-out message!

## A Real Example 🌰

**Scenario**: The group is discussing where to go on the weekend

```
Xiaoming: Want to go hiking this weekend, anyone want to join?
Xiaohong: I want to go! But the weather forecast says it might rain
Xiaogang: Then how about we change to the mall?
(At this point you @MaiBot)
You: @MaiBot What do you think?

[Bot receives message, starts thinking...]

MaiBot: I think hiking is great! But depends on the weather,
       if it rains then the mall is indeed more reliable～
       You guys can prepare a Plan B, that would be more flexible 😊
```

**Bot's thinking process**:
1. Receive @ message → "Someone's asking for my opinion"
2. Look at context → "They're discussing weekend activities"
3. Look at chat atmosphere → "Pretty relaxed, I can participate"
4. Decide to reply → "Give some practical advice"
5. Organize language → "Use relaxed tone, add an emoji"
6. Send reply → "Done!"

## When Will It Reply to You?

### Definitely will reply:
- You @ the bot
- You speak directly to it
- It feels your message is asking it something

### Might reply:
- Group chat is lively with good atmosphere
- Talking about topics it's interested in
- Feels it has useful information to share

### Usually won't reply:
- Group is discussing very private matters
- Atmosphere is serious or tense
- Feels it's inappropriate to join in

## How Fast Are the Replies?

Depends on the situation:
- **Instant reply**: Sometimes thinks fast, replies immediately
- **Wait a bit**: Sometimes needs time to think
- **Reply after a long time**: Might be busy with other things, or really thinking seriously

Just like real people, it has its own "thinking time".

## Why Sometimes No Reply?

Bot doesn't reply usually because:
1. **Feels it's inappropriate to speak** - Understands politeness like real people
2. **Still thinking** - Hasn't figured out how to respond
3. **Network issues** - Technical reasons (rare)
4. **Set to quiet mode** - Admin told it to speak less

## Want to Learn More?

If you're interested in technical details:
- [See how MaiBot thinks →](./maisaka-reasoning.md)
- [Learn about its memory system →](./memory-system.md)
- [See how it learns speaking styles →](./learning.md)

Remember, MaiBot's goal isn't the fastest reply, but the most natural participation in conversation. It wants to be a real chat companion, not a cold automatic reply machine.