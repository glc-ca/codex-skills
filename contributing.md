# Contributing

## Goal

Keep this repository as a clean catalog of GLC-owned Codex skills.

## Structure

- Place stable skills in `skills/.curated/<skill-name>/`.
- Place early-stage skills in `skills/.experimental/<skill-name>/`.
- Use lowercase kebab-case for skill folder names.

## Minimum skill contents

Each skill folder must include:

- `SKILL.md` with required frontmatter:
  - `name`
  - `description`

Recommended optional files:

- `agents/openai.yaml`
- `scripts/`
- `references/`
- `assets/`

## Pull request checklist

- Skill folder is in the correct lane (`.curated` or `.experimental`).
- `SKILL.md` is concise and task-specific.
- Instructions are deterministic where reliability matters.
- Any scripts are runnable and clearly referenced from `SKILL.md`.
- No secrets or credentials are committed.
