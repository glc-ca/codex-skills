# AGENTS

## Scope

This repository is the source of truth for GLC-owned Codex skills.

## Repository Layout

- Put stable skills in `skills/.curated/<skill-name>/`.
- Put in-progress skills in `skills/.experimental/<skill-name>/`.
- Use lowercase kebab-case for skill folder names.

## Minimum Skill Requirements

Each skill folder must include:

- `SKILL.md`
- YAML frontmatter with at least:
  - `name`
  - `description`

Recommended optional contents:

- `agents/openai.yaml`
- `scripts/`
- `references/`
- `assets/`

## Create or Update Skills

Use `$skill-creator` to create or update skills. Follow `$CODEX_HOME/skills/.system/skill-creator/SKILL.md`.

## Authoring Rules

- Keep `SKILL.md` concise and task-specific.
- Put trigger conditions in the `description` field.
- Make instructions deterministic where reliability matters.
- If scripts are included, ensure they are runnable and referenced from `SKILL.md`.
- Do not commit secrets or credentials.

## Validation Checklist

Before opening a PR:

- Confirm the skill is in the correct lane (`.curated` vs `.experimental`).
- Confirm naming is lowercase kebab-case.
- Run quick validation:

```bash
python3 /Users/justinkropp/.codex/skills/.system/skill-creator/scripts/quick_validate.py <skill-directory>
```
