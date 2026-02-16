---
name: azure-rg-sandbox
description: Use this skill when Codex needs safe Azure CLI execution scoped to one workspace sandbox resource group. Primary workflow is `sandbox az ...`, with one-time bootstrap via `sandbox az init --location LOCATION`, plus health checks (`sandbox status`) and teardown (`sandbox destroy --yes`).
---

# Azure RG Sandbox

Use `sandbox az ...` for all Azure operations.

If sandbox state is missing in this workspace, run:
- `sandbox az init --location <azure-location>`

Then retry the same `sandbox az ...` command.

## Commands

- `sandbox az init --location <azure-location> [--subscription <id>] [--resource-group <name>] [--recreate] [--json]`
  - Initializes or recreates the workspace sandbox boundary.
- `sandbox az <azure-cli-args...>`
  - Runs scoped Azure CLI commands using sandbox auth.
  - Never auto-initializes; if state is missing, it tells you to run `sandbox az init --location <location>` first.
- `sandbox status [--json]`
  - Reports sandbox health and drift.
- `sandbox destroy --yes [--json]`
  - Tears down sandbox resources and local sandbox state.

## Notes

- State file is workspace-local: `.codex/azure-rg-sandbox/state.json`.
- `sandbox az init` maintains `~/.codex/rules/azure-rg-sandbox.rules` and does not modify `~/.codex/rules/default.rules`.
- If that rules file changes, restart Codex to fully apply prompt-free behavior.
