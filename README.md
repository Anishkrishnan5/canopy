# canopy

> Working-memory infrastructure for long-running AI agents.

canopy is an open-source runtime for managing an AI agent's working memory.

Instead of continually appending every tool call, observation, retrieved document, and reasoning step into an ever-growing context window, canopy manages how context moves between active, passive, summarized, and archived states.

The goal is simple:

**help agents think with the right context at the right time.**

---

## Why?

Long-running AI agents accumulate context quickly.

During a multi-step task, an agent may generate:

* tool outputs
* error messages
* retrieved documents
* previous attempts
* intermediate plans
* branch-specific state
* stale observations
* summaries
* user constraints

Most agent systems handle this by either:

* appending everything to the prompt
* periodically summarizing old context
* retrieving memories from storage

These approaches help, but they do not fully solve the working-memory problem.

The hard question is not only:

> Where should information be stored?

It is:

> What should the model actively see before the next reasoning step?

canopy is built around that question.

---

## What is canopy?

canopy is a working-memory runtime that sits between an agent and the LLM.

Before each LLM call, canopy decides:

* what must stay pinned
* what is currently active
* what should become passive
* what should be summarized
* what should be archived
* what should be rehydrated only when needed

This allows long-running agents to reason over extended tasks without dragging their entire execution history into every prompt.

---

## Core idea

canopy treats context as a scarce runtime resource.

The model's context window is similar to working memory: limited, expensive, and easy to pollute.

canopy manages context using a memory hierarchy:

* **Pinned** — critical information that should always remain visible
* **Active** — the current working set for the agent's next step
* **Passive** — potentially useful context that is not currently needed
* **Summary** — compressed historical context
* **Archive** — full raw history, available for rehydration when necessary

---

## Architecture

```text
                 Agent
                   │
                   ▼
           canopy Runtime
                   │
        Working Memory Manager
                   │
        ┌──────────┼──────────┐
        │          │          │
     Pinned      Active     Passive
        │          │          │
        └──────────┼──────────┘
                   │
          Context Window Builder
                   │
                   ▼
                  LLM
```

canopy does not replace your agent framework, model provider, vector database, or storage layer.

Instead, canopy manages the layer between available context and the model's limited attention.

---

## What canopy is not

canopy is not:

* a vector database
* a generic memory database
* a full agent framework
* a replacement for LangGraph, OpenAI Agents SDK, or other runtimes
* a RAG framework
* a prompt compression wrapper

canopy is focused on one responsibility:

> managing an agent's working context during long-running execution.

---

## Early roadmap

### Phase 0 — Architecture and concepts

Define the problem, core abstractions, architecture, roadmap, and research notes.

### Phase 1 — Core models

Implement the foundational data structures:

* `Memory`
* `MemoryState`
* `ContextWindow`
* `TokenBudget`
* `Branch`

### Phase 2 — Working-memory store

Implement an in-memory store for recording and updating agent context.

### Phase 3 — Context allocation

Implement deterministic rules for deciding what enters the next context window.

### Phase 4 — Token budgeting

Track context size and enforce token budgets.

### Phase 5 — Runtime wrapper

Wrap a simple agent loop so canopy can record events and assemble context before each LLM call.

### Phase 6 — Summarization

Add summarization for inactive or oversized context.

### Phase 7 — Branch-aware memory

Support separate branches of investigation, each with its own working context.

### Phase 8 — Inspector and benchmarks

Add visibility into what canopy included, excluded, summarized, or rehydrated.

Benchmark against a vanilla append-only agent loop.

---

## Status

🚧 Early development.

canopy is currently in architecture and design phase.
