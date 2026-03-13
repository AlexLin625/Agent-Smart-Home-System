# tests

## Scope
- This file governs `room-agent/tests/`.

## Purpose
- Keep tests aligned with the rebuilt LangGraph runtime, not the older digital-human README flow.

## Current Coverage
- `graph/`: routing, classification fallback, and end-to-end graph execution.
- `tools/`: MCP tool selection and invocation normalization.

## Editing Rules
- Any behavior change in `graph/`, `tools/`, `integrations/`, or `config/` should usually land with a test change here.
- Prefer narrow tests around public behavior over tests that lock in implementation trivia.
- Preserve no-credentials and no-tool fallback coverage. Those are intentional product behaviors.

## Commands
- Graph tests: `uv run --with pytest --with pytest-asyncio pytest tests/graph -q`
- Tool tests: `uv run --with pytest --with pytest-asyncio pytest tests/tools -q`
