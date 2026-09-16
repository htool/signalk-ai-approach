# Server vs plugin

Do not load `signalk-server` source “for orientation.” If a **client contract is undefined** (no path, no plugin API, no documented behaviour), do not invent a private workaround and treat it as the design. Wrapping that hole in a plugin facade is [hiding friction](principles.md).

## When the hole might belong in the server

Ask this before coding:

> Other plugins or the server itself already need the same fact
> (example: App Store and server-update already probe npm).
> Should this be a `signalk-server` capability, not a one-plugin hack?

If **yes or unsure:** stop and **ask the human** whether to open or wait on an upstream issue or RFC. Do not open it yourself unless they say so. If other plugins will share the contract, see below.

If **no** (boat-local, one plugin, no shared probe): implement in the plugin only.

## Shared contracts: RFC, not a plugin spec

If **other plugins or apps will target the same schema or API** (a new resource type, a v2 API, a path many writers need), that contract is not a plugin ADR. A private REST API, token mint, or custom resource schema shipped as fait accompli is [hiding friction](principles.md).

Ask the human whether to open or wait on an upstream RFC. Do not open it yourself unless they say so.

| What | Where |
|---|---|
| Schema, data model, “is this a resource or a v2 API?” | RFC on [SignalK/specification](https://github.com/SignalK/specification) ([CONTRIBUTING](https://github.com/SignalK/specification/blob/master/CONTRIBUTING.md); [issue 264](https://github.com/SignalK/specification/issues/264) is the format) |
| Server mechanics (`app.*`, Resources ids/POST, `getFeatures()`) | Issue on [SignalK/signalk-server](https://github.com/SignalK/signalk-server) |

A custom resource type can ship without a spec change. That is not consensus. Implementing against a **draft RFC** is the workaround below. Treating the plugin’s schema as the spec is how competing private APIs proliferate.

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
