# Azure RG Sandbox State and Health

## State File Location

- `.codex/azure-rg-sandbox/state.json`

## `sandbox az` Init Precondition

- `sandbox az ...` is execution-only and never auto-initializes.
- The command requires `.codex/azure-rg-sandbox/state.json` in the current workspace.
- If state is missing, `sandbox az` exits non-zero and prints plain text telling you to run:
  - `sandbox az init --location <location>`

## State Schema (v1)

```json
{
  "version": 1,
  "workspace_path": "/absolute/workspace/path",
  "workspace_id": "workspace-slug-a1b2c3d4e5",
  "subscription_id": "00000000-0000-0000-0000-000000000000",
  "tenant_id": "00000000-0000-0000-0000-000000000000",
  "resource_group_name": "codex-sbx-workspace-slug-a1b2c3d4e5",
  "resource_group_scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/codex-sbx-workspace-slug-a1b2c3d4e5",
  "location": "canadacentral",
  "service_principal_app_id": "00000000-0000-0000-0000-000000000000",
  "service_principal_object_id": "00000000-0000-0000-0000-000000000000",
  "service_principal_display_name": "codex-sbx-workspace-slug-a1b2c3d4e5",
  "service_principal_client_secret": "plaintext-secret",
  "created_at_utc": "2026-02-16T00:00:00Z",
  "updated_at_utc": "2026-02-16T00:00:00Z"
}
```

## Health Checks Performed by `sandbox status`

1. `state-schema`
   - Validate required fields and state version.
   - Confirm `workspace_path` matches current workspace.
2. `resource-group-exists`
   - Confirm bound resource group still exists in bound subscription.
3. `service-principal-exists`
   - Confirm service principal app ID resolves in Entra ID.
4. `owner-assignment`
   - Confirm at least one `Owner` role assignment exists at exact resource-group scope.
5. `assignment-boundary`
   - Confirm service principal has no role assignments outside the sandbox scope prefix.
6. `sandbox-auth`
   - Confirm sandbox `AZURE_CONFIG_DIR` is authenticated and on bound subscription.
   - Attempt re-authentication with stored secret when needed.
7. `gitignore-protection`
   - Confirm workspace `.gitignore` contains `.codex/azure-rg-sandbox/`.

## Remediation Model

- Emit `healthy=false` when any check fails.
- Include specific remediation hints in output.
- Recommend `sandbox az init --location <location> --recreate` for unrecoverable drift.

## Codex Rule File Management

- `sandbox az init` maintains a dedicated Codex rules file at:
  - `~/.codex/rules/azure-rg-sandbox.rules`
- Managed content is one wrapper prefix rule for the installed sandbox script path.
- The skill does not modify:
  - `~/.codex/rules/default.rules`
- If the managed file is created or updated, the script prints a one-line note recommending a Codex restart so rule scanning fully applies.

## Known Azure Timing Behavior

- New service principals and fresh RBAC assignments can take time to propagate.
- During propagation, login may fail with messages like `No subscriptions found`.
- `sandbox az init` should retry sandbox service-principal login with backoff before concluding failure.

## Known Entra Credential Behavior

- If `az ad sp create-for-rbac` reuses an existing application/service principal by name, the returned `password` field may not always be usable for login.
- `sandbox az init` should attempt login with the returned secret first.
- On `AADSTS7000215` or equivalent invalid-secret failures during init, rotate credentials with `az ad app credential reset` and retry login before failing.
