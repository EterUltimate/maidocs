---
title: Memory Configuration
---

# Memory Configuration

The `[memory]` section in `bot_config.toml` controls MaiBot's memory system, including long-term memory retrieval, person fact writeback, chat summary writeback, and feedback correction functions.

MaiBot's memory system works like human memory — it can remember things about you, summarize conversations, and learn from past interactions. The system automatically extracts key information from conversations and stores it persistently.

## Configuration Items Overview

```toml
[memory]
global_memory = false
enable_memory_query_tool = true
memory_query_default_limit = 5
person_fact_writeback_enabled = true
chat_summary_writeback_enabled = true
chat_summary_writeback_message_threshold = 12
chat_summary_writeback_context_length = 50
feedback_correction_enabled = false
feedback_correction_window_hours = 12.0
feedback_correction_check_interval_minutes = 30
feedback_correction_batch_size = 20
feedback_correction_auto_apply_threshold = 0.85
feedback_correction_max_feedback_messages = 30
feedback_correction_prefilter_enabled = true
feedback_correction_paragraph_mark_enabled = true
feedback_correction_paragraph_hard_filter_enabled = true
feedback_correction_profile_refresh_enabled = true
feedback_correction_profile_force_refresh_on_read = true
feedback_correction_episode_rebuild_enabled = true
feedback_correction_episode_query_block_enabled = true
feedback_correction_reconcile_interval_minutes = 5
feedback_correction_reconcile_batch_size = 20

[[memory.global_memory_blacklist]]
platform = ""
item_id = ""
rule_type = "group"
```

## Basic Memory Functions

### global_memory

- **Type**: `bool`
- **Default**: `false`
- **Description**: Whether to allow memory retrieval to perform global queries in chat records

When enabled, tools like `query_memory` will ignore the current chat flow's `chat_id` restriction when retrieving memories, searching in all chat flows. This means MaiBot can reference information learned in private chats when in group chats.

::: warning
Enabling global memory may involve privacy issues. It is recommended to exclude sensitive chat flows with `global_memory_blacklist`.
:::

### global_memory_blacklist

- **Type**: `list[TargetItem]`
- **Default**: `[]`
- **Description**: Global memory blacklist

When `global_memory` is enabled, chat flows in the blacklist will not be included in global retrieval. Each `TargetItem` contains:

| Field | Type | Description |
|------|------|------|
| `platform` | `str` | Platform identifier, empty means no restriction |
| `item_id` | `str` | User/Group ID, empty means no restriction |
| `rule_type` | `"group"` \| `"private"` | Chat flow type |

### enable_memory_query_tool

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to enable the built-in long-term memory retrieval tool `query_memory`

MaiBot's Planner can actively retrieve long-term memory through the `query_memory` tool. When closed, Planner will not be able to actively query the memory database.

### memory_query_default_limit

- **Type**: `int`
- **Default**: `5`
- **Range**: `1 - 20`
- **Description**: Default return count of the `query_memory` tool

Controls the maximum number of results returned for each memory retrieval.

## Memory Writeback

MaiBot has two automatic writeback services that automatically extract important information into long-term memory during conversations.

### person_fact_writeback_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to automatically extract and write back person facts to long-term memory after sending replies

When enabled, MaiBot will analyze conversation content after each reply, extract factual information about users (such as preferences, identity, etc.) and write it to long-term memory. This is the core mechanism for MaiBot to "get to know you better and better".

### chat_summary_writeback_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to automatically write back chat summaries to long-term memory during chat based on message window

When enabled, MaiBot will automatically generate chat summaries and write them to long-term memory after conversations accumulate to a certain amount. This helps MaiBot review previous chat content in subsequent conversations.

### chat_summary_writeback_message_threshold

- **Type**: `int`
- **Default**: `12`
- **Range**: `≥ 1`
- **Description**: Message window threshold for automatic chat summary writeback

When new messages accumulated in the chat flow reach this threshold, it triggers a chat summary writeback.

### chat_summary_writeback_context_length

- **Type**: `int`
- **Default**: `50`
- **Range**: `1 - 500`
- **Description**: Number of message entries to look back from the chat flow when automatically writing back chat summaries

Controls the context window size referenced when generating summaries. Larger values provide more context but consume more tokens.

## Feedback Correction System

The feedback correction system is an advanced feature of the memory system that corrects stored incorrect memories by monitoring user feedback in subsequent conversations.

::: tip
Feedback correction is disabled by default (`feedback_correction_enabled = false`), it is recommended to enable it after becoming familiar with basic functions.
:::

### feedback_correction_enabled

- **Type**: `bool`
- **Default**: `false`
- **Description**: Whether to enable feedback-driven delayed memory correction tasks

### feedback_correction_window_hours

- **Type**: `float`
- **Default**: `12.0`
- **Range**: `≥ 0.1`
- **Description**: Feedback window duration (hours)

Starting from the time when `query_memory` is executed, user feedback messages within the specified time range will be included in the correction analysis.

### feedback_correction_check_interval_minutes

- **Type**: `int`
- **Default**: `30`
- **Range**: `≥ 1`
- **Description**: Polling interval for feedback correction scheduled tasks (minutes)

### feedback_correction_batch_size

- **Type**: `int`
- **Default**: `20`
- **Range**: `1 - 200`
- **Description**: Maximum processing quantity per round of correction tasks

### feedback_correction_auto_apply_threshold

- **Type**: `float`
- **Default**: `0.85`
- **Range**: `0.0 - 1.0`
- **WebUI Control**: Slider, step 0.01
- **Description**: Minimum confidence threshold for automatically applying correction actions

When the correction confidence reaches this threshold, automatically apply correction operations without manual confirmation. Higher thresholds mean stricter requirements.

### feedback_correction_max_feedback_messages

- **Type**: `int`
- **Default**: `30`
- **Range**: `1 - 200`
- **Description**: Maximum number of user feedback messages within the window used for each correction task

### feedback_correction_prefilter_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to enable correction pre-filtering

Pre-filtering can reduce unnecessary model calls and lower costs.

### feedback_correction_paragraph_mark_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to write "corrected old fact" marks for affected memory paragraphs

### feedback_correction_paragraph_hard_filter_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to hard filter paragraphs with stale marks in user queries

When enabled, memory paragraphs marked as stale will not appear in query results.

### feedback_correction_profile_refresh_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to add affected person profiles to the refresh queue after feedback correction

### feedback_correction_profile_force_refresh_on_read

- **Type**: `bool`
- **Default**: `true`
- **Description**: When person profile is in dirty queue, whether to force refresh instead of reusing old snapshots when reading

### feedback_correction_episode_rebuild_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: Whether to add affected memory sources to the episode rebuild queue after feedback correction

### feedback_correction_episode_query_block_enabled

- **Type**: `bool`
- **Default**: `true`
- **Description**: When episode source is in rebuild queue, whether to block user queries

### feedback_correction_reconcile_interval_minutes

- **Type**: `int`
- **Default**: `5`
- **Range**: `≥ 1`
- **Description**: Polling interval for feedback correction second-stage consistency coordination tasks (minutes)

### feedback_correction_reconcile_batch_size

- **Type**: `int`
-- **Default**: `20`
- **Range**: `1 - 200`
- **Description**: Batch size for processing profile/episode queues in second-stage consistency per round

## Memory System Workflow

```
User Message → MaiBot Reply
                ↓
    ┌───────────────────────────┐
    │  person_fact_writeback     │ ← Extract person facts
    │  (After each reply)        │
    └───────────────────────────┘
                ↓
    ┌───────────────────────────┐
    │  chat_summary_writeback    │ ← Generate chat summary
    │  (Every N messages)        │
    └───────────────────────────┘
                ↓
    ┌───────────────────────────┐
    │  feedback_correction       │ ← Detect and correct wrong memories
    │  (Optional, scheduled poll)│
    └───────────────────────────┘
                ↓
         Long-term Memory Engine
```

## Complete Configuration Example

```toml
# Basic configuration (recommended for beginners)
[memory]
global_memory = false
enable_memory_query_tool = true
memory_query_default_limit = 5
person_fact_writeback_enabled = true
chat_summary_writeback_enabled = true
chat_summary_writeback_message_threshold = 12
chat_summary_writeback_context_length = 50

# Feedback correction (advanced feature, enable as needed)
[memory]
feedback_correction_enabled = false
# feedback_correction_window_hours = 12.0
# feedback_correction_check_interval_minutes = 30
# feedback_correction_batch_size = 20
# feedback_correction_auto_apply_threshold = 0.85
```