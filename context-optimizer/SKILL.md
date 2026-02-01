---
name: context-optimizer
description: "Optimize OpenClaw token usage via context monitoring and 20% threshold reset strategies. Use when: (1) Monitoring context usage, (2) Managing token costs for long conversations, (3) Checking session health/quota, (4) Optimizing Gemini/LLM performance via periodic /new resets while preserving core memory."
---

# Context Optimizer

This skill implements a strategy to manage the linear/exponential growth of token costs in long conversations by monitoring context usage and triggering resets at optimal thresholds (20%).

## Core Strategy: The 20% Threshold

LLMs are stateless; history must be re-sent every turn. To prevent cost explosions:
1. **Monitor**: Check context usage before major tasks or when requested.
2. **Alert**: Notify the user when usage reaches **20%** of the model's limit.
3. **Reset**: Use the `/new` command (or channel equivalent) to clear context while relying on `MEMORY.md` and `IDENTITY.md` for continuity.

## Status Check

When the user asks for "status" or "quota", use the `session_status` tool to retrieve and present:
- **Current Context Size**: (e.g., 20k / 1.0m)
- **Quota/Usage Percentage**: (e.g., 2%)
- **Reset Time**: When the current token window or rate limit resets.

## Preserving Continuity

Before resetting with `/new`:
1. **Sync Memory**: Ensure significant learnings are written to `MEMORY.md` or `memory/YYYY-MM-DD.md`.
2. **Identity**: Ensure `IDENTITY.md` and `SOUL.md` are up to date.
3. **Task Status**: Summarize the current task status so it can be resumed immediately after reset.

## Commands

- **`/new` or `/reset`**: The primary command to clear the current conversation history. 
    - **When to use**: Always suggest or trigger a reset when context usage reaches the 20% threshold.
    - **Agent Instruction**: When you determine a reset is needed, clearly instruct the user: "Context usage is at [X]%. I recommend starting a new session with `/new` to optimize tokens. I have synced our memory."
    - **Post-Reset**: Upon waking in a new session, immediately read `MEMORY.md` and the most recent `memory/YYYY-MM-DD.md` to resume context.

## LIMITS

- **Code Generation & Sub-agents**: This strategy cannot eliminate all costs. If you frequently ask the assistant to build code (especially via sub-agents), token consumption will still be significant. Sub-agent usage contributes heavily to the overall quota.

## Reference Material

- See [STRATEGY.md](references/STRATEGY.md) for the detailed token optimization theory.
