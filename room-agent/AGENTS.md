# room-agent

## Scope
- This file governs the `room-agent/` subtree.
- Prefer a deeper `AGENTS.md` if one exists in a child directory.

## Read This First
- Treat [docs/2026.3 整体系统设计.md](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/docs/2026.3%20整体系统设计.md) and [docs/2026.3 RoomAgent 需求边界.md](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/docs/2026.3%20RoomAgent%20需求边界.md) as product/design truth.
- Treat [docs/LANGGRAPH_REBUILD_MIGRATION_PLAN.md](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/docs/LANGGRAPH_REBUILD_MIGRATION_PLAN.md) as migration intent and staging plan.
- Do not trust [README.md](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/README.md) as current runtime documentation. It describes an older digital-human workflow and is out of date for this package.

## Current Runtime Status
- The active runtime is the LangGraph rebuild under `app/`, `graph/`, `integrations/`, `tools/`, `config/`, and `tests/`.
- The only real entrypoint today is [app/main.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/app/main.py). It runs the graph once from CLI.
- Current implemented flow is:
  `classify_intent -> simple_chat | mcp_workflow | finalize`.
- `mcp_workflow` currently does only `choose_tool -> call_tool`. It is intentionally minimal.
- Device requests are recognized, but they do not execute device control yet. They end in `handoff_required`.
- A2A gateway, long-running runtime, and production orchestration are not implemented in this subtree yet.
- `core/room_agent/` contains older MQTT, beacon, and device abstractions. They are not wired into `app/main.py` or the current graph.

## Work Priorities
- Extend the LangGraph path before touching legacy `core/room_agent/`.
- Keep deterministic routing and structured outputs. Do not add free-form graph state mutations.
- Preserve graceful fallback when no LLM credentials are present.
- When adding behavior, update or add tests in `tests/` in the same change.

## Fast Context Map
- [app/main.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/app/main.py): CLI entrypoint and graph invocation.
- [graph/builder.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/builder.py): main graph topology.
- [graph/state.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/state.py): shared state contract.
- [graph/nodes/classify_intent.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/nodes/classify_intent.py): intent routing and heuristic fallback.
- [graph/subgraphs/mcp_workflow.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/subgraphs/mcp_workflow.py): minimal MCP execution path.
- [integrations/llm_provider.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/integrations/llm_provider.py): OpenAI-compatible provider factory.
- [integrations/mcp_client.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/integrations/mcp_client.py): file-based MCP client adapter.
- [tools/mcp_tools.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/tools/mcp_tools.py): MCP tool selection and invocation normalization.
- [config/settings.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/config/settings.py): config loading with `env > yaml > defaults`.

## Commands
- Install deps: `uv sync`
- Run the graph once: `uv run python app/main.py "你好"`
- Run with a config file: `uv run python app/main.py "今天天气怎么样" --config config/room_agent.yaml`
- Run focused tests: `uv run --with pytest --with pytest-asyncio pytest tests/graph tests/tools -q`

## Editing Rules
- Keep new work inside the rebuild path unless the task is explicitly about legacy MQTT/device/beacon code.
- If you change graph routing, update `graph/builder.py`, the affected node/subgraph, and matching tests together.
- If you add new external capability, put transport/client code in `integrations/` and decision logic in `graph/` or `tools/`.
- If you add new runtime config, wire it through [config/settings.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/config/settings.py) rather than reading environment variables ad hoc.
- Preserve `result`, `error`, and `trace` shapes unless you update all consumers and tests.
