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

Ask the human whether to open or wait on an upstream RFC. Do not open it yourself unless they say so. If they say open it, also draft a short Discord announce and a GitHub @mention list (see below).

Before drafting, read [Decisions](decisions.md) and [RFCs](rfcs.md). Decisions are the ADR log (accepted or rejected). RFCs are the open issues, wherever they were filed (specification, server, or a plugin). An open issue is not the spec. A merged PR is, even when the issue was left open.

If the RFC is missing from [RFCs](rfcs.md), add a row: date, issue link, title. Do not paste the body. The issue is the reference. When it is accepted or rejected, write the outcome in [Decisions](decisions.md) and delete the row.

| What | Where |
|---|---|
| Schema, data model, “is this a resource or a v2 API?” | RFC on [SignalK/specification](https://github.com/SignalK/specification) ([CONTRIBUTING](https://github.com/SignalK/specification/blob/master/CONTRIBUTING.md); [issue 264](https://github.com/SignalK/specification/issues/264) is the format) |
| Server mechanics (`app.*`, Resources ids/POST, `getFeatures()`) | RFC or issue on [SignalK/signalk-server](https://github.com/SignalK/signalk-server) |
| A contract that starts in one plugin and other plugins must share | RFC on that plugin’s repo, and a row in [RFCs](rfcs.md) |

A custom resource type can ship without a spec change. That is not consensus. Implementing against a **draft RFC** is the workaround below. Treating the plugin’s schema as the spec is how competing private APIs proliferate.

## After the RFC exists: announce and notify

The GitHub issue is the contract. Discord is a pointer, not a second copy of the design. Do **not** post to Discord or open the RFC yourself unless the human says so. Draft the texts; they paste.

**Discord announce** — a few lines, then the issue URL. No 26 KB paste.

```
RFC: <one-line title>
<issue URL>
<one sentence: who else should care and why>
```

**Where to post:** [Signal K Discord](https://discord.gg/uuZrwz4dCS). Not `#help` (that is support; the invite lands there). Use the development discussion channel. Server docs link [this channel](https://discord.com/channels/1170433917761892493/1170433918592368684) as community chat. Tell the human that place by name; they post.

**GitHub notify** — @mention a few people who already own the overlap, using their **GitHub** usernames (repo owner, CODEOWNERS, or named maintainers of the two or three plugins / the server module). Not a `git shortlog` of `signalk-server`. Not invented handles.

Put the @mentions in the RFC body, or as a **first comment** whose only job is the ping (better on a long RFC so they get notified without wading through the design):

```
Flagging for eyes: @alice (resources API), @bob (signalk-logbook).
```

Same cap as [retrieve overlap](#how-to-retrieve-overlap-narrowly): a handful of named people, then stop.

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
