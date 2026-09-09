# Plugin kit

Drop these in the plugin repo. Keep them short. They are for the next agent, not a blog.

| File | Role |
|---|---|
| `AGENTS.md` | Index. Link [the approach](https://github.com/htool/signalk-ai-approach). Rules. Next slice. |
| `README.md` | Human + agent scope card: job, in, out, not |
| `docs/architecture.md` | Current behaviour (update when code lands) |
| `docs/adr/00N-….md` | One decision per file. Status: accepted → implemented |
| `docs/features.md` | Ordered slices. Done-when. **Tests on every feature slice.** |
| `docs/known-gaps.md` | Honest leftovers |
| `skills/<domain>/SKILL.md` | Do / do not for this plugin |

Docs-only slices (the kit itself) do not change `index.js`. Feature slices do, and they include tests.
