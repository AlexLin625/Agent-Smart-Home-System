# core/room_agent

## Scope
- This file governs `room-agent/core/room_agent/`.

## Status
- This directory is legacy infrastructure from the pre-LangGraph implementation.
- It contains MQTT client management, beacon helpers, device abstractions, and room-domain models.
- It is not currently invoked by [app/main.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/app/main.py) or [graph/builder.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/graph/builder.py).

## Use It Carefully
- Do not add new top-level product features here unless the task is explicitly about legacy migration or device/MQTT integration.
- When you need existing device or MQTT semantics, prefer extracting small reusable pieces into the new runtime instead of growing this legacy path further.
- If you migrate code out of here, keep behavior-oriented tests in the new destination directory.

## Subdirectories
- `mqtt/`: external broker connection and topic routing.
- `devices/`: registry/controller abstractions for room devices.
- `models/`: device state and MQTT payload models.
- `beacon/`: ESP32 beacon-related helpers.

## Migration Guidance
- Treat this directory as a reference implementation for domain behavior, not as the default place for new orchestration code.
- If you wire legacy code into the rebuild, document the coupling in the parent [room-agent/AGENTS.md](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/AGENTS.md).
