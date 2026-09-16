# Engineering principles

Judgment rules for plugin work. Retrieval, workflow, CI, and PRs stay on their own pages.

If a slice would violate a principle, change the design or ask. Do not add a layer that makes the violation look tidy.

## Do not hide friction behind a new abstraction

> Do not add a new abstraction to hide friction in an existing one. Use that friction to find what the shared interface is missing. Specialize only when reuse is wrong, not when it is messy.

**Friction is a signal.** An awkward `app.*` call, a path you wish existed, or a retry loop because the server has no “internet is up” API, is missing shared behaviour. Name the hole. Ask whether it belongs on `signalk-server` ([Server vs plugin](server-boundary.md)). Do not wrap the awkwardness in a facade, helper module, or second protocol so the rest of the plugin looks clean.

**Reuse first, specialize on purpose.** Two plugins that need the same fact should not each grow a private stack. One plugin with a different job (a converter that must emit NMEA, a device that must own a bus address) should implement that job directly — not by wrapping a messy shared path in a lookalike API. Messy is not “wrong reuse.” Wrong reuse is a different contract.

**Same rule for the agent.** If the kit files cannot implement the slice, [fix the docs](retrieve.md). Do not grow `AGENTS.md` or paste `signalk-server` `src/` to hide that the map is thin.

| Do | Do not |
|---|---|
| Call the existing path / `app.*` and live with the call site | Invent `ServerFacade`, `NmeaHelper`, or a private JSON dialect to tidy the call |
| Put a one-function seam and a known-gaps entry while the server decision is open | Scatter `fetch('https://…')` or copy another plugin’s paths as the spec |
| Implement NMEA only in a converter or emulator | Emit HEX PGNs from a path-only plugin because conversion looked reusable |
| Specialize the module that has a different job | Fork a shared interface behind a wrapper because the shared one is messy |
