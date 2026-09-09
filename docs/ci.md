# CI — tests on GitHub for every PR

This matches Signal K practice. Do not treat `npm test` as a laptop-only habit.

## What upstream does

**Server** — [`signalk-server` `test.yml`](https://github.com/SignalK/signalk-server/blob/master/.github/workflows/test.yml) runs on every `pull_request` and on push to `master`.

**Plugins (simple)** — e.g. `signalk-to-nmea2000`: `pull_request` + push to `master`, Node matrix, `npm install`, `npm test`.

**Plugins (official reusable workflow)** — documented at [Plugin CI/CD](https://github.com/SignalK/signalk-server/blob/master/docs/develop/plugins/ci.md). One caller file in the plugin:

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

## Cerbo / Pi

`plugin-ci.yml` can emulate armv7 (Venus OS / Cerbo) and run on arm64 (Pi). Hardware (CAN, serial) is not in GitHub-hosted runners; that stays a boat or self-hosted runner test.
