# Pull requests

Follow [Signal K server contributing](https://github.com/SignalK/signalk-server/blob/master/CONTRIBUTING.md) even for a plugin:

- Branch from latest parent `master`/`main`.
- One logical change. Split unrelated topics.
- **Never change version numbers** in the PR; maintainers publish.
- Title and body: why and how, not a tour of the diff. No AI fluff. If default config meaning changes (e.g. 1 km → 1 NM), say so.
- Rebase onto parent; do not merge parent into the feature branch unless the project asks for merge commits.
- `npm test` and CI green before “ready for human review.”

Open the PR against the **parent** repository (`sbender9/…`, `SignalK/…`). Merging only to your fork is for boat testing; it is not the contribution.

## After you open it

Do not treat `gh pr create` as the end of the job.

- **Check that tests passed** on the PR (`gh pr checks`, GitHub Actions). A green local `npm test` is not enough. If CI is red, fix and push.
- **Read review comments** (maintainer, reviewer, or bot). If a comment needs a code change, make it and update the PR. If it does not (wrong, already addressed, needs a human call), say so on the thread. Do not leave comments unread.
