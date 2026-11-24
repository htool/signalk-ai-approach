# Signal K decisions (ADRs)

Settled outcomes of Signal K RFCs. One section per decision: accepted or rejected. Checked against merged pull requests and the schema or server docs, not against the issue still being open.

This is the ADR log for the project. Open RFCs are not listed here; they are in [RFCs](rfcs.md). When an RFC is accepted or rejected, add the section here and delete its row there. The GitHub issue stays the reference. Do not paste the thread.

This is not a plugin ADR directory. A plugin ADR is one file under that plugin’s `docs/adr/` ([plugin kit](plugin-kit.md)).

Numbered RFCs start in 2016. Earlier choices (SI units on each key, the full model plus deltas, `self`, multiple sources kept side by side) are already in the specification book: [keys](https://github.com/SignalK/specification/blob/master/mdbook/src/keys.md), [data model](https://github.com/SignalK/specification/blob/master/mdbook/src/data_model.md), [multiple values](https://github.com/SignalK/specification/blob/master/mdbook/src/data_model_multiple_values.md). This page does not restate them.

## How a specification change is decided

[CONTRIBUTING](https://github.com/SignalK/specification/blob/master/CONTRIBUTING.md) says a substantial change takes the form of an RFC. [Issue 264](https://github.com/SignalK/specification/issues/264) is the format: Summary, Motivation, Detailed design, Drawbacks, Alternatives, Unresolved questions.

Two rules from the record:

1. **A schema cleanup can be a pull request.** On [#267](https://github.com/SignalK/specification/issues/267) (2016-10-06) Tim Mathews wrote that the PR is enough, and RFCs should stay reserved for larger structural changes.
2. **The spec is the merged schema and book, not the issue state.** [#442](https://github.com/SignalK/specification/issues/442) is still open. PUT has been in the book since [PR 507](https://github.com/SignalK/specification/pull/507) (merged 2018-10-28). An open issue is not a licence to implement the thread.

The labels collided. Two different proposals were both called “RFC 0004”: [#269](https://github.com/SignalK/specification/issues/269) (wildcards, merged) and [#293](https://github.com/SignalK/specification/issues/293) (entertainment, still draft). Cite the issue number.

Opening an RFC is a human decision. See [Server vs plugin](server-boundary.md). Tracking the open ones is [RFCs](rfcs.md).

## Accepted

### Subscription paths use `*`

Status: accepted. [Issue 269](https://github.com/SignalK/specification/issues/269) / [PR 269](https://github.com/SignalK/specification/pull/269), merged 2016-10-13. Also labeled “RFC0004”.

Subscriptions match paths with `*` ([subscription protocol](https://github.com/SignalK/specification/blob/master/mdbook/src/subscription_protocol.md)). A wildcard in the middle (`propulsion/*/oilTemperature`) or at the end (`propulsion/port/*`) is the spec. JsonPath is not required.

### Source metadata is grouped by transport

Status: accepted as a cleanup. [Issue 267](https://github.com/SignalK/specification/issues/267) / [PR 266](https://github.com/SignalK/specification/pull/266), merged 2016-10-09. Also labeled “RFC 0002”.

Redundant source fields were removed. NMEA 2000 fields sit under `n2k`. `productID` is a number. The current shape is [`sources.json`](https://github.com/SignalK/specification/blob/master/schemas/groups/sources.json). The RFC text also suggested making `src` a number; the schema on `master` still has `n2k.src` as a string. Follow the schema.

Later source work ([PR 458](https://github.com/SignalK/specification/pull/458), [PR 548](https://github.com/SignalK/specification/pull/548)) extended this. It did not reopen the cleanup.

### Electrical device types stay separate

Status: the generalisation was rejected. [Issue 268](https://github.com/SignalK/specification/issues/268), closed 2016-10-27. Also labeled “RFC0003”.

The proposal was one `charger` object plus a `chargerType` enum (AC, alternator, solar, wind, …). The thread voted to keep a group per device type, so device-specific fields have a home. The issue text records that decision.

What landed afterwards, as its own PR and not as that RFC: [PR 375](https://github.com/SignalK/specification/pull/375) added `electrical.alternators`, `electrical.solar`, and shared `chargerQualities`. [`electrical.json`](https://github.com/SignalK/specification/blob/master/schemas/groups/electrical.json) has `batteries`, `inverters`, `chargers`, `alternators`, `solar`, and `ac` as siblings.

Where an alternator also belongs relative to `propulsion` was not decided. That question is still [specification#331](https://github.com/SignalK/specification/issues/331), listed in [RFCs](rfcs.md).

### A change request is a PUT

Status: accepted in the book. [Issue 442](https://github.com/SignalK/specification/issues/442) is still open. [PR 507](https://github.com/SignalK/specification/pull/507) merged 2018-10-28. Also labeled “RFC 0010”.

[PUT](https://github.com/SignalK/specification/blob/master/mdbook/src/put.md) asks the server to take an action (anchor light, autopilot heading, stereo input). A delta update reports the current value. The two are different messages.

The `source` field is optional. If it is omitted and the path has more than one source, the server responds with HTTP 400. Request/response details are in [request/response](https://github.com/SignalK/specification/blob/master/mdbook/src/request_response.md). The delta form that wraps `put` in an array is deprecated; the object form is the one to send. The issue was left open after the chapter landed. Follow the chapter.

### Signed athwartships quantities are positive to starboard

Status: the two outliers are fixed in the schema. [Issue 685](https://github.com/SignalK/specification/issues/685) is still open. [PR 680](https://github.com/SignalK/specification/pull/680) merged 2026-10-03.

`sensors.*.fromCenter` and `steering.autopilot.target.windAngleApparent` / `windAngleTrue` are **+ve to starboard, −ve to port**, same as rudder angle, roll, yaw, and rate of turn. Treat a port-positive reading of `fromCenter` as a bug, not as the spec. The issue stays open; the schema is the decision. Do not list it again under [RFCs](rfcs.md).

## Outside this log

Server 2.x APIs (Course, Resources, Autopilot, Weather, History, Radar) were specified as OpenAPI on the server, not as rows here. A missing v2 API is still a server question, not a plugin schema ([Server vs plugin](server-boundary.md)). Open server RFCs are indexed in [RFCs](rfcs.md).
