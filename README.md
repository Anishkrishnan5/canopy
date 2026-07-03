# canopy

> Runtime context allocation for long-running AI agents.

canopy is an open-source runtime that manages an AI agent's working memory.

Instead of continually appending every tool call, observation, and reasoning step into an ever-growing context window, canopy dynamically allocates context by deciding what should remain active, what should be summarized, what should become passive, and what should be retrieved only when necessary.

The goal is to make long-running agents:

- More reliable
- More efficient
- Less expensive
- Easier to inspect and debug

---

## Why?

Modern AI agents often accumulate thousands of tokens of stale context during long-running tasks.

Current approaches typically rely on:

- Append-only conversation history
- Periodic summarization
- Retrieval from long-term memory

canopy approaches the problem differently.

Instead of treating context as a log, canopy treats it as **working memory**.

Before every LLM call, canopy assembles the smallest useful context window from the agent's current task state.

---

## Core Concepts

Every memory belongs to one of several categories:

- 📌 Pinned — Never removed
- 🌿 Active — Currently relevant
- 🍃 Passive — Available if needed
- 📝 Summary — Compressed historical context
- 📦 Archive — Full raw history

---

## Architecture

```
                Agent
                  │
                  ▼
          canopy Runtime
                  │
        Context Allocation
                  │
                  ▼
                LLM
```
