# Architecture

## Design principle

Design for the worst network first. Every feature must work with local storage and store-and-forward sync. Richer behavior is layered on top when connectivity is available.

## Components

| Layer | Technology | Purpose |
|---|---|---|
| Client | Progressive Web App (service worker, IndexedDB, offline map tiles) | Offline-capable interface for public users |
| Transport | HTTPS sync, SMS gateway, Bluetooth Low Energy (BLE) peer relay | Three independent delivery paths |
| Backend | Node.js or Python API, PostgreSQL with PostGIS, append-only event log | Geo queries, sync, audit trail |
| Alert ingest | CAP feed poller and parser | Imports official alerts with signatures |
| Responder console | Web dashboard with live map and priority queue | Role-gated operations view |

## Data flow

```text
Official CAP feed ──► Alert ingest ──► Alerts store ──► Sync API ──► Client cache
Public user ──► Client (IndexedDB) ──► Sync queue ──► Sync API ──► Request store
                         │                                    │
                         └── SMS / BLE relay (when no internet) ┘
Responder console ◄── Priority engine ◄── Request store
```

## Alert handling

| Tier | Source | Display rule |
|---|---|---|
| 1 | Official CAP feed, signed | Shown immediately with source and time |
| 2 | Responder-verified report | Shown after a human confirms |
| 3 | Crowd report | Labeled "unverified" until N independent reports within a radius and time window |

Every alert shows its source, issue time, received time, and verification status.

## Help request lifecycle

Each request moves through a fixed set of states:

`received → queued → acknowledged → dispatched → resolved`

Each transition is appended to a hash-chained log. Each entry includes the previous entry's hash, so any edit breaks the chain and can be detected.

## Priority score

```text
Priority = Severity × 0.4 + Vulnerability × 0.25 + Time waiting × 0.2 + Corroboration × 0.15
```

| Factor | Source |
|---|---|
| Severity | Category (trapped or medical is high) and life-threat keywords |
| Vulnerability | Self-declared flags such as infant, elderly, disability, or medical dependency |
| Time waiting | Increases with age, so no request is forgotten |
| Corroboration | Nearby requests or official reports in the same area |

Responders can override a score, but the override and its reason are logged.

## Offline and sync strategy

1. **Store and forward:** Every write goes to IndexedDB first, then syncs.
2. **SMS fallback:** Compact message format, for example `HELP M3 4P 12.97,77.59 TRAPPED`, parsed by a gateway.
3. **BLE relay:** Phones pass queued messages to nearby phones. (Simulated in the demo.)
4. **Payload budget:** Messages stay under 160 bytes where possible.
5. **Conflict handling:** Append-only events with timestamps and device IDs, so merges do not lose data.

## Privacy controls

- No account required. Users have pseudonymous local IDs.
- Public map views use coarsened cells (about 500 m).
- Family check-ins are end-to-end encrypted. The server stores ciphertext only.
- Requests have a time-to-live (TTL) and are deleted or anonymized after resolution.
- Aggregate public statistics use differential privacy noise.

## Security

- Ed25519 signatures on reports for origin verification.
- Rate limits per device to reduce spam.
- Role-based access for responder tools.
- Encryption at rest and in transit.

## Metrics and how they are measured

| Metric | Measurement |
|---|---|
| Alert timeliness | Timestamp from CAP issue time to device display |
| Location accuracy | Distance error between submitted and reference coordinates, with accuracy radius shown |
| Request traceability | Full hash-chained history available for every request |
| Usability | Time for a first-time user to submit a request; System Usability Scale (SUS) survey |

## Known limitations

- SMS gateway and BLE relay are simulated in the current demo.
- Corroboration thresholds need tuning on real data.
- Priority scoring is rule-based, not learned.
