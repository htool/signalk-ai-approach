# Retrieve, don’t dump

Do not load `signalk-server` into the agent context “for orientation.” A plugin’s contract is paths, deltas, and maybe NMEA via converters — not the server’s Express internals.

Use six layers, on the **plugin** (or a small set of plugins), in this order:

| Layer | In the plugin | Role |
|---|---|---|
| 1. Index | `AGENTS.md` | What to read first. Link this approach. No server `src/`. |
| 2. Map | `docs/architecture.md` | Config → subscribe → Signal K paths → outputs |
| 3. Contract | README scope card | Job, in, out, not. v1 paths unless a real v2 API exists |
| 4. Decisions | `docs/adr/*` | Locked choices (units, encodings, option gating) |
| 5. Gaps | `docs/known-gaps.md` | Bugs and out of scope. Do not “fix” these in the current slice |
| 6. Skills | `skills/*/SKILL.md` | How to change this domain without breaking clients |

If the agent cannot implement a slice from those files, **fix the docs** — do not grow the prompt or paste the server.

If the slice needs a **server** behaviour that is not a documented path or `app.*` method, follow [Server vs plugin](server-boundary.md). Ask the human; do not dump `signalk-server`. Do not crawl the plugin registry by keyword.

v1 Signal K is the data model. Plugins write deltas (`app.handleMessage`). They do not emit NMEA unless they are a converter (e.g. `signalk-to-nmea2000`) or they emulate a bus address (`simpleCan`). HEX PGN is last resort when fields are unknown.
