# OpenClaw Token Optimization Strategy

## The Problem: Stateless History
LLM context grows linearly or exponentially. The 10th message costs roughly 10x more than the 1st message because the entire history is re-sent.

## The Solution: 20% Threshold Reset
By resetting the context window at 20% usage, we stay in the "cheap and fast" zone of the model's operation.

### 1. Monitoring
- **Trigger**: Before large operations or on user request.
- **Data Source**: `session_status` tool.

### 2. Logic & Alerts
- **Threshold**: 20%.
- **Action**: Proactive notification to the user recommending `/new`.

### 3. Physical Reset
- **Action**: User sends `/new`.
- **Result**: Context counter returns to ~0.
- **Safety**: Memory and Persona (Identity) are preserved via permanent files, preventing "AI amnesia".

## Implementation in OpenClaw
This strategy is uniquely effective in OpenClaw because of the robust file-based memory system (`AGENTS.md`, `MEMORY.md`) which survives session resets.
