# integrations

## Scope
- This file governs `room-agent/integrations/`.

## Purpose
- Keep external system adapters here.
- Do not put business routing, prompts, or product-specific policy in this directory.

## Current Modules
- [llm_provider.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/integrations/llm_provider.py):
  wraps LangChain `ChatOpenAI` in OpenAI-compatible mode.
- [mcp_client.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/integrations/mcp_client.py):
  loads MCP server config from JSON and exposes a thin `get_tools()` interface.

## Editing Rules
- Preserve the adapter boundary: inputs and outputs should stay simple enough for `graph/` and `tools/` to consume without transport knowledge.
- Keep LLM providers returning plain text only. JSON mode should still return text containing JSON, not parsed objects.
- Keep `create_llm_provider()` returning `None` when credentials are missing. The graph relies on that fallback path.
- Add new provider options through [config/settings.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/config/settings.py) first.
- If you add a new MCP transport or adapter behavior, keep file parsing and connection normalization in [mcp_client.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/integrations/mcp_client.py).

## Validation
- Prefer `tests/graph/` plus `tests/tools/` because these adapters are exercised through graph and tool behaviors.
