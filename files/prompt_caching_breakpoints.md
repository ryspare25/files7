# Prompt Caching: Cache Ordering and Breakpoints

## Fixed Hierarchy

Cache breakpoints are evaluated top-down in this fixed order:

```
1. Tools (if provided)
2. System Prompt (if provided)
3. Messages
```

Each layer accepts a `cache_control: {type: "ephemeral"}` breakpoint independently. A cache miss at any layer invalidates everything below it.

## Partial Hit Scenario

Tools unchanged + system prompt changed:

| Layer | Result |
|---|---|
| Tools | **cache read** — hit, ~10% of normal token cost |
| System prompt | **cache write** — miss, full processing + stored |
| Messages | processed fresh (below the miss point) |

## Cost Model

- **Cache hit:** ~10% of normal input token price
- **Cache miss:** full token processing cost + write cost
- You only pay full price for tokens below the lowest cache hit

## Operational Rule

Put the most *stable* content highest in the hierarchy:

- **Tools** — almost never change → cache first, maximum reuse
- **System prompt** — changes occasionally → cache second
- **Messages** — change every turn → cache only if there's a large static preamble before conversation history

## Key Use Case

Agentic loops with large tool definitions + static system prompt. After the first request, subsequent turns hit the tools and system prompt cache, cutting input costs dramatically. Only the new message content is processed fresh each turn.
