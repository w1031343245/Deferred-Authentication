# Deferred Authentication

An agent skill for keeping long-running tasks focused when authentication, permissions, publishing, deployment, or upload steps block progress.

Authentication is not the goal; it is a capability. When a required capability blocks repeatedly, preserve the original goal by producing a resumable artifact instead of letting the task become a validation loop for that capability.

## Install

Install this repository root as the skill package:

```bash
python install-skill-from-github.py --repo w1031343245/Deferred-Authentication --path . --name deferred-authentication
```

## Package Layout

- `SKILL.md`: main skill instructions.
- `agents/interface.yaml`: skill interface and adapter compatibility metadata.
- `manifest.json`: package metadata.
- `references/fallback-options.md`: deferred fallback examples.
