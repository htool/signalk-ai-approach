# CI — tests on GitHub for every PR

This matches Signal K practice. Do not treat `npm test` as a laptop-only habit.

## How Signal K already tests

Yes — tests are meant to run on GitHub for PRs.

| Repo | What runs |
|---|---|
| [signalk-server](https://github.com/SignalK/signalk-server/blob/master/.github/workflows/test.yml) | On `pull_request` and push to `master`: install, build, `npm test` (Node 22 and 24). |
| Typical plugins (e.g. [signalk-to-nmea2000](https://github.com/sbender9/signalk-to-nmea2000/blob/master/.github/workflows/main.yml)) | On `pull_request` and push to `master`: `npm install` and `npm test` on a Node matrix. |
| Official plugin path | Reusable workflow [`plugin-ci.yml`](https://github.com/SignalK/signalk-server/blob/master/.github/workflows/plugin-ci.yml). Docs: [Plugin CI/CD](https://github.com/SignalK/signalk-server/blob/master/docs/develop/plugins/ci.md). Default `test-command` is `npm test`. Also validates schema, lifecycle, and App Store install even if you have no tests. |

A plugin with only `"test": "echo Error"` will not catch regressions. A plugin with `node --test` (or similar) plus a workflow **will** fail the PR when tests fail — that is the intended gate.

## What to add

**Plugins (official reusable workflow)** — one caller file in the plugin:

```yaml
name: SignalK Plugin CI

on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]

jobs:
  test:
    uses: SignalK/signalk-server/.github/workflows/plugin-ci.yml@master
```

Default `test-command` is `npm test`. If `package.json` has a real test script, a failing suite **fails the PR**. The same workflow also checks plugin structure, `schema`, start/stop, and App Store-style install — useful even before you have many tests.

Prefer this caller over a one-off `npm test` job when you can. If `plugin-ci.yml` is too strict for an old plugin (e.g. routes registered in `start()` instead of `registerWithRouter`), start with the simple `npm test` workflow, list the CI failures in `known-gaps.md`, and fix them in their own slice.

## Floor

- `package.json` `"test"` must run the suite (not `echo Error`).
- A workflow on `pull_request` must run that script.
- Feature work is not done until CI would catch a regression of that feature.
- After opening a PR, confirm the workflow actually passed. Review comments: [Pull requests](prs.md).

## Cerbo / Pi

`plugin-ci.yml` can emulate armv7 (Venus OS / Cerbo) and run on arm64 (Pi). Hardware (CAN, serial) is not in GitHub-hosted runners; that stays a boat or self-hosted runner test.
