# Workflow

## 1. Sync the fork first

If you work on a GitHub fork, **update it from the parent before you measure your delta.** A fork that is 10 commits behind will mix “already upstream” with your work. That is the expensive mistake.

- Keep unique fork work on a backup branch (and push it) before `gh repo sync --force`.
- Do not add an `upstream` remote if the user forbids git-config changes; fetch the parent by URL or use `gh repo sync`.
- Recode onto current parent `index.js`. Do not cherry-pick old commits onto a tree that grew APIs (`resendAlerts`, v2 routes, …).

## 2. Decide what belongs upstream

Summarize fork-only behaviour vs synced parent. Ask what to reapply. Drop boat-local hacks and `bla` commits. One logical PR per topic.

If a missing contract looks like a **shared server** concern, follow [Server vs plugin](server-boundary.md) before coding the plugin workaround.

## 3. Docs commit, then code

On a branch from **synced** `master`/`main`:

1. ADR + features slice (**no plugin code**).
2. Code + **tests** for that slice (`npm test` green).
3. PR to origin. Do not bump `package.json` version (maintainers publish). Then [check CI and review comments](prs.md#after-you-open-it); update the PR if code must change.

## 4. Tests are part of the feature

Not a later chore. Done-when includes tests. Extract logic that does not need a running server (thresholds, message text, resend gating) into `lib/` so `node --test` (or the plugin’s runner) can run in CI.

Live AIS inject on a boat is useful, not a substitute for `npm test`.

## 5. Then CI on GitHub

See [CI](ci.md). If tests are not in GitHub Actions, a green local run is invisible on the PR.
