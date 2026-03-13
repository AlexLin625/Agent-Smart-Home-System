# config

## Scope
- This file governs `room-agent/config/`.

## Purpose
- Keep runtime configuration centralized and predictable.

## Current Rules
- [settings.py](/Users/hongjielin/Code/grad-labs/Agent-Smart-Home-System/room-agent/config/settings.py) is the single source of truth for config loading logic.
- Precedence is `environment variables > yaml file > code defaults`.
- YAML files in this directory define room-agent runtime defaults, not secrets.
- Secrets belong in `.env` or the shell environment, not in committed YAML files.

## Editing Rules
- Add new config fields to the Pydantic models and wire them through `load_settings()`.
- Do not read `os.getenv()` from random runtime modules if the value belongs in settings.
- Keep default values usable for local development when possible.
- If a config key changes behavior in `graph/` or `integrations/`, update this directory and the affected tests together.
