---
title: Features
---

# Features

MaiBot (Maimai) is more than just a chatbot - she's a digital life form dedicated to interacting in authentic human style. Here are her core capabilities.

<div class="feature-cards">

<div class="feature-card">

### 💬 Intelligent Message Pipeline

Complete processing pipeline from message reception to final response, supporting Hook interception, command distribution, filtering checks, and flexible routing.

[Learn about Message Pipeline →](/en/manual/features/message-pipeline)

</div>

<div class="feature-card">

### 🧠 Maisaka Reasoning Engine

Multi-round internal reasoning system based on tool calls. Planner decision-making, Timing Gate rhythm control, automatic interruption and retry, making conversation flow naturally.

[Learn about Maisaka →](/en/manual/features/maisaka-reasoning)

</div>

<div class="feature-card">

### ❤️ Long-term Memory System

A-Memorix memory engine provides knowledge graphs, conversation summaries, and character profiles. Automatic write-back mechanism lets Maimai continuously accumulate understanding of you.

[Learn about Memory System →](/en/manual/features/memory-system)

</div>

<div class="feature-card">

### 📖 Expression & Jargon Learning

Automatically extracts expression styles and group jargon from conversations, infers meanings through LLM and continuously improves, making Maimai more like people around you.

[Learn about Learning System →](/en/manual/features/learning)

</div>

<div class="feature-card">

### 😊 Emoji System

Automatic emoji recognition based on VLM, emotion tag generation and intelligent selection, making conversations more vivid.

[Learn about Emoji System →](/en/manual/features/emoji-system)

</div>

<div class="feature-card">

### 🔌 MCP Integration

Supports Model Context Protocol, connects to external tool servers, unlimited expansion of Maimai's capability boundaries.

[Learn about MCP →](/en/manual/features/mcp)

</div>

</div>

<style>
.feature-cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 16px;
  margin-top: 24px;
}
.feature-card {
  border: 1px solid var(--vp-c-divider);
  border-radius: 8px;
  padding: 20px;
  transition: border-color 0.25s, box-shadow 0.25s;
}
.feature-card:hover {
  border-color: var(--vp-c-brand);
  box-shadow: 0 2px 12px var(--vp-c-brand-soft);
}
.feature-card h3 {
  margin-top: 0;
  font-size: 1.1em;
}
.feature-card a {
  font-size: 0.9em;
}
</style>