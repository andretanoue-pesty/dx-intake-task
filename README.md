# DX Asana intake

Agent skill for creating and updating DX Operations website tasks in Asana: Simple Requests (bug, incident and recurring QA variants), Delivery Workstreams with their stages, and release QA tasks.

Portable: the procedure uses capability names, and every workspace-specific ID lives in one file, `skills/dx-asana-intake/references/config.json`.

## Layout

- `skills/dx-asana-intake/SKILL.md` - entry point: routing, pre-flight, procedure, do-nots.
- `skills/dx-asana-intake/references/config.json` - projects, sections, assignee, custom fields and options, clients, delivery stages, template IDs.
- `skills/dx-asana-intake/references/field-rules.md` - field rules for every task.
- `skills/dx-asana-intake/references/asana-capabilities.md` - capability-to-tool mapping and connector rules.
- `skills/dx-asana-intake/references/simple-request.md`, `delivery-workstream.md`, `qa-task.md` - the three templates.
- `skills/dx-asana-intake/agents/openai.yaml` - interface metadata.

## Using it

Ask the assistant to file a request with the skill, for example: "Use $dx-asana-intake to file this DX website request in Asana." It asks for anything missing, shows a draft, and creates the task only after you confirm.

## Maintaining it

- Live Asana templates win over these files. The pre-flight step checks them before any write.
- Config values are dated (verified 2026-10-27). Update `config.json` when DX fields, clients or templates change.
- To use another Asana workspace, copy `config.json` and replace the values, keeping the same keys.
