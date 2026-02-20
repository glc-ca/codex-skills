# GLC Codex Skills

Custom Codex skills for GLC, published from this repository.

This repo starts intentionally empty and is the source of truth for your team-owned skills.

## Repository layout

- `skills/.curated/` for stable team-approved skills.
- `skills/.experimental/` for in-progress or trial skills.

Each skill should live in its own folder and include at least `SKILL.md`.

## Sandbox Skill Status

The codex sandbox skill (`skills/.experimental/azure-sandbox/`) is experimental and is not currently functioning.

## Install skills with `$skill-installer`

Install directly from this repository by URL.

Curated skill example:

```text
$skill-installer install https://github.com/glc-ca/codex-skills/tree/main/skills/.curated/<skill-name>
```

Experimental skill example:

```text
$skill-installer install https://github.com/glc-ca/codex-skills/tree/main/skills/.experimental/<skill-name>
```

You can also install from repo/path form:

```text
$skill-installer install --repo glc-ca/codex-skills --path skills/.curated/<skill-name>
```

After installing a skill, restart Codex to pick up new skills.

## Add a new skill

1. Choose a lane: `.curated` or `.experimental`.
2. Create a folder, for example `skills/.curated/my-skill/`.
3. Add `SKILL.md` with frontmatter (`name`, `description`) and instructions.
4. Optionally add `agents/openai.yaml`, `scripts/`, `references/`, or `assets/`.
5. Commit and push to `main`.

## License

This repository is licensed under MIT. Individual skills may include additional license notes where needed.
