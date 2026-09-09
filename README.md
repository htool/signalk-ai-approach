# Signal K AI approach

A way to use an AI coding agent on [Signal K](https://signalk.org) **plugins** without dumping the server into context, and without shipping untested PRs.

This is a **standalone public repo** (docs only). It is not a plugin and not a fork of `signalk-server`. Point a plugin’s `AGENTS.md` here; do not copy these pages into every plugin tree.

**If you are an agent:** read this README, then the plugin’s own `AGENTS.md`. Do not load `signalk-server` source unless a client contract is actually undefined.

## Why

Signal K server is large. A plugin is small. Agents that start from the server tree waste context and invent APIs. Agents that skip tests and CI ship regressions. This approach keeps the plugin as the unit of work and matches how Signal K already reviews and tests code.

## How Signal K already tests

Yes — tests are meant to run on GitHub for PRs.

| Repo | What runs |
|---|---|
| [signalk-server](https://github.com/SignalK/signalk-server/blob/master/.github/workflows/test.yml) | On `pull_request` and push to `master`: install, build, `npm test` (Node 22 and 24). |
| Typical plugins (e.g. [signalk-to-nmea2000](https://github.com/sbender9/signalk-to-nmea2000/blob/master/.github/workflows/main.yml)) | On `pull_request` and push to `master`: `npm install` and `npm test` on a Node matrix. |
| Official plugin path | Reusable workflow [`plugin-ci.yml`](https://github.com/SignalK/signalk-server/blob/master/.github/workflows/plugin-ci.yml). Docs: [Plugin CI/CD](https://github.com/SignalK/signalk-server/blob/master/docs/develop/plugins/ci.md). Default `test-command` is `npm test`. Also validates schema, lifecycle, and App Store install even if you have no tests. |

A plugin with only `"test": "echo Error"` will not catch regressions. A plugin with `node --test` (or similar) plus a workflow **will** fail the PR when tests fail — that is the intended gate.

## Contents

1. [Retrieve, don’t dump](docs/retrieve.md) — six layers instead of the server tree
2. [Plugin kit](docs/plugin-kit.md) — `AGENTS.md`, ADRs, features, gaps, skills
3. [Workflow](docs/workflow.md) — sync the fork, docs commit, then code + tests
4. [CI](docs/ci.md) — GitHub Actions on every PR
5. [Pull requests](docs/prs.md) — Signal K PR hygiene

## Quick start for a plugin

1. Put an `AGENTS.md` in the plugin that points here and lists the local kit files.
2. Every **feature slice** includes tests in its done-when. Put testable logic in a small module (e.g. `lib/`) and run `npm test` before the code commit.
3. Add `.github/workflows/signalk-ci.yml` that calls Signal K’s reusable plugin CI (see [CI](docs/ci.md)). Floor: `npm test` on `pull_request`.
4. Open PRs against **upstream** (`sbender9/…`, `SignalK/…`), not only your fork. Sync the fork from parent **before** you treat your commits as the delta.

## Licence

Apache-2.0, same family as much of Signal K.
