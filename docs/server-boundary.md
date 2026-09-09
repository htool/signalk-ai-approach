# Server vs plugin

Do not load `signalk-server` source “for orientation.” If a **client contract is undefined** (no path, no plugin API, no documented behaviour), do not invent a private workaround and treat it as the design.

## When the hole might belong in the server

Ask this before coding:

> Other plugins or the server itself already need the same fact
> (example: App Store and server-update already probe npm).
> Should this be a `signalk-server` capability, not a one-plugin hack?

If **yes or unsure:** stop and **ask the human** whether to open or wait on an upstream issue. Do not open a `signalk-server` issue yourself unless they say so.

If **no** (boat-local, one plugin, no shared probe): implement in the plugin only.

## Meanwhile

Ship a **plugin workaround** so the slice can land. In the same slice:

1. Put a **known-gaps** (or ADR) entry: what is missing on the server, the workaround, and **check back for the decision**.
2. Prefer a seam that can later call `app.*` or a path (e.g. “renew when internet is up” behind one function). Do not scatter `fetch('https://…')` as the architecture.
3. Do not depend on unrelated plugins’ paths (OpenWrt LTE, Netgear, …) as if they were the spec.

## How to retrieve overlap (narrowly)

If you must confirm the hole is real:

1. **Server:** [DeepWiki](https://deepwiki.com) / DeepWiki MCP (`ask_question` on `SignalK/signalk-server`) or **one** related server module (e.g. App Store npm fetch). Not `src/`. Quote the existing behaviour in the ask to the human.
2. **Other plugins:** at most **two or three** you can already name. Look up `github_url` in the [plugin registry](https://signalk.org/signalk-plugin-registry/) if needed, then DeepWiki those repos.

Then stop reading server and plugin trees.

## Do not crawl the plugin registry

The [plugin registry](https://signalk.org/signalk-plugin-registry/) is a **scoreboard** (name, score, badges, `github_url`), not a topic index. It does not list npm keywords or descriptions. Discovery into it is a single keyword: `signalk-node-server-plugin` — that is every plugin.

Do **not** “smart-filter” 600 plugins by keywords or synonym expansion (`internet`, `cloud`, `mqtt`, `lte`, …). App Store categories (`signalk-category-cloud`, …) are optional and coarse; they are not a search engine.

Use the registry as a **phone book** for the named hits above. Cap: three other plugins, then ask the human.

## Check back

When the human later says the server grew the API, replace the workaround in its own slice. Until then, do not poll GitHub for that issue on every turn.
