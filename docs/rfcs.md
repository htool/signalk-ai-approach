# Open Signal K RFCs

Index of RFCs that are **not** decided yet. The GitHub issue is the text. This page is a pointer list: one row, no copy of the design.

Settled outcomes live in [Decisions](decisions.md). That file is the ADR log. When an RFC is accepted or rejected, write the outcome there and delete the row here.

Plugin ADRs (`docs/adr/` inside a plugin) are a third thing. They record a choice inside one codebase. A contract other plugins will share is an RFC, even when the issue is filed on a plugin repo.

## How to find them

RFCs are not all in `SignalK/specification`. File one in the repo that owns the contract: the specification for the data model, `signalk-server` for a server API, a plugin repo when that plugin is where the shared contract starts.

Search before proposing or implementing a shared contract:

```
gh search issues --owner SignalK "RFC in:title" --state open
```

Older specification RFCs also carry the `RFC` label. A title that only cites some other standard (for example “RFC 6901”) is not a Signal K RFC. Skip it.

This list lags unless someone updates it. If the search hits an issue that is not listed here and is not already in [Decisions](decisions.md), add a row in the same change. Do that before writing code against it.

## Already decided, issue still open

Do not add these back. GitHub still shows them open. The ADR is in [Decisions](decisions.md).

| Issue | ADR |
|---|---|
| [specification#442](https://github.com/SignalK/specification/issues/442) | A change request is a PUT |
| [specification#685](https://github.com/SignalK/specification/issues/685) | Signed athwartships quantities are positive to starboard |

## Specification

| Since | Issue |
|---|---|
| 2016-09 | [specification#264](https://github.com/SignalK/specification/issues/264) RFC 0001: equipment associations |
| 2016-11 | [specification#293](https://github.com/SignalK/specification/issues/293) RFC 0004: entertainment (draft). No `entertainment` group in the schema. [#296](https://github.com/SignalK/specification/pull/296) closed unmerged. |
| 2016-11 | [specification#294](https://github.com/SignalK/specification/issues/294) RFC 0005: streaming over a range of transports (draft) |
| 2016-11 | [specification#297](https://github.com/SignalK/specification/issues/297) RFC 0006: device data model |
| 2016-12 | [specification#305](https://github.com/SignalK/specification/issues/305) RFC 0007: no-server Signal K (experimental) |
| 2017-02 | [specification#331](https://github.com/SignalK/specification/issues/331) RFC 0008: alternator placement. `electrical.alternators` is already decided ([Decisions](decisions.md)). The link to `propulsion` is not. |
| 2018-02 | [specification#441](https://github.com/SignalK/specification/issues/441) RFC 0009: digital switching |
| 2018-03 | [specification#452](https://github.com/SignalK/specification/issues/452) RFC 0011: target values |
| 2026-02 | [specification#673](https://github.com/SignalK/specification/issues/673) RFC: DSC notification paths |
| 2026-10 | [specification#688](https://github.com/SignalK/specification/issues/688) RFC: depth transducer offsets |
| 2026-10 | [specification#694](https://github.com/SignalK/specification/issues/694) RFC: a `location` property for notifications |

## Server

| Since | Issue |
|---|---|
| 2026-03 | [signalk-server#2407](https://github.com/SignalK/signalk-server/issues/2407) RFC: Signal K Database API |
| 2026-03 | [signalk-server#2417](https://github.com/SignalK/signalk-server/issues/2417) RFC: naming for provider and API interfaces |
| 2026-03 | [signalk-server#2508](https://github.com/SignalK/signalk-server/issues/2508) RFC: Device Manager |
| 2026-09 | [signalk-server#3037](https://github.com/SignalK/signalk-server/issues/3037) RFC: writing tracks through the Track API |
| 2026-09 | [signalk-server#3048](https://github.com/SignalK/signalk-server/issues/3048) RFC: LogEntries resource |
| 2026-09 | [signalk-server#3077](https://github.com/SignalK/signalk-server/issues/3077) RFC: structured waypoints |
| 2026-10 | [signalk-server#3127](https://github.com/SignalK/signalk-server/issues/3127) RFC: radar and camera targets linked to AIS vessels |
| 2026-10 | [signalk-server#3168](https://github.com/SignalK/signalk-server/issues/3168) RFC: Trips resource |

## Plugins and converters

| Since | Issue |
|---|---|
| 2026-03 | [n2k-signalk#312](https://github.com/SignalK/n2k-signalk/issues/312) RFC: map PGN 129285 route data to Course API paths |
| 2026-10 | [sailsconfiguration#13](https://github.com/SignalK/sailsconfiguration/issues/13) RFC: sail limits, polars per sail plan and sea state |

Do not implement a row as if it were the spec. A local workaround stays in the plugin’s known-gaps until the issue is decided ([Server vs plugin](server-boundary.md)).
