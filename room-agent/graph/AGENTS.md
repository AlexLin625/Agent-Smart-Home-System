# graph

## Scope
- This file governs `room-agent/graph/`.

## Purpose
- Own the LangGraph runtime contract for the rebuilt room agent.
- Keep orchestration here; keep provider details in `integrations/`; keep tool wrappers in `tools/`.

## Current Graph
- Main graph is defined in [builder.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/builder.py).
- State contract is defined in [state.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/state.py).
- Current route set is fixed:
  `simple_chat`, `mcp`, `device`, `error`.
- Current `device` route is a placeholder path to `finalize`; no device execution node exists yet.
- Current MCP branch is the subgraph in [subgraphs/mcp_workflow.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/subgraphs/mcp_workflow.py).

## Editing Rules
- Return partial state updates from nodes; do not rebuild the whole state manually.
- Preserve `trace` appends for every meaningful node action.
- Keep routing decisions explicit and testable. Do not hide routing inside side-effectful nodes.
- Keep `intent`, `task`, `mcp`, `result`, and `error` payloads structured for downstream inspection.
- If you introduce a new route, update:
  [builder.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/builder.py),
  [nodes/task_router.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/nodes/task_router.py),
  and matching tests under `tests/graph/`.

## Node-Specific Notes
- [nodes/classify_intent.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/nodes/classify_intent.py):
  Prefer predictable JSON outputs and preserve heuristic fallback when LLM calls fail.
- [nodes/simple_chat.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/nodes/simple_chat.py):
  Keep replies short and side-effect free.
- [nodes/finalize.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/nodes/finalize.py):
  This is the public result shaper. Changes here are externally visible.

## Validation
- `uv run --with pytest --with pytest-asyncio pytest tests/graph -q`
