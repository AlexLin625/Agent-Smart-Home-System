# tools

## Scope
- This file governs `room-agent/tools/`.

## Purpose
- Hold tool-facing service code that the graph can call directly.
- Current focus is MCP tool discovery, selection, argument shaping, and result normalization.

## Current Contract
- [mcp_tools.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/tools/mcp_tools.py) exposes:
  `list_tools()`, `describe_tools()`, `choose_tool()`, and `invoke_tool()`.
- Standard call result shape is:
  `success`, `tool_name`, `result`, `error`, `raw`.

## Editing Rules
- Keep tool-selection heuristics deterministic when no LLM provider is available.
- Keep invocation results normalized even when upstream tools return unusual payloads.
- Do not let graph nodes talk to raw MCP clients directly; route that through `MCPToolService`.
- If you add argument mapping logic, make it schema-aware and cover it with tests under `tests/tools/`.

## Validation
- `uv run --with pytest --with pytest-asyncio pytest tests/tools -q`
