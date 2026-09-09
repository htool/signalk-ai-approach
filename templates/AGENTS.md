# Agents

This plugin: <!-- one sentence: job, and what it does not do. -->

Follow [Signal K AI approach](https://github.com/htool/signalk-ai-approach). Do not copy those pages into this tree. Do not load `signalk-server` `src/` unless a client contract is undefined.

## Fill this file

Read the approach, then **this** repo. Replace the placeholders with facts about this codebase. Do not invent APIs, overlaps, or bans.

Show the filled `AGENTS.md` to the human. **Wait for confirmation** before treating it as law.

## Then the rest of the kit

After this index is confirmed, add the other files from [plugin kit](https://github.com/htool/signalk-ai-approach/blob/main/docs/plugin-kit.md) (`docs/architecture.md`, `docs/adr/`, `docs/features.md`, `docs/known-gaps.md`, `skills/…`) the same way: draft from this repo, show the human, wait.

**Also check** the tree for other AI-oriented markdown (`CLAUDE.md`, `GEMINI.md`, `llms.txt`, extra `AGENTS.md`, `.cursorrules`, `.cursor/rules/**`, `.github/copilot-instructions.md`, skill dumps). If any of those conflict with this index or the approach (dump the server, skip tests, a second growing prompt), **ask the human** before you add or rewrite kit files. Do not silently merge, delete, or prefer the other file.

Keep this file an index. Detail lives in `docs/` and skills, loaded on demand.

## Read first

1. [README.md](README.md) — scope card (job, in, out)
2. [docs/architecture.md](docs/architecture.md)
3. [docs/adr/](docs/adr/) — locked decisions
4. [docs/features.md](docs/features.md) — next pending slice only
5. [docs/known-gaps.md](docs/known-gaps.md) — do not invent these here
6. Then plugin source — never import `signalk-server` `src/`

## Overlap

| Repo | Role |
| --- | --- |
| This plugin | <!-- what this repo owns --> |
| <!-- neighbour --> | <!-- what it owns instead --> |

If a slice cannot be done from these files, fix the docs. Do not grow the prompt.
