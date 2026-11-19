# Beacon: Offline-First Disaster Response System

Beacon delivers verified alerts, safe-location maps, prioritized geotagged help requests, and family safety check-ins. It works with low or no connectivity.

Built for **NEXUS 0: The Innovation Arena** (Nexus Club, MITE). Winner.

## Problem

Disasters break networks, and people lose access to official alerts, shelters, and ways to ask for help. Beacon is designed for the worst network conditions first, then adds richer features when connectivity is available.

## Key features

- **Verified alerts:** Official CAP feeds shown immediately. Crowd reports are labeled "unverified" until corroborated.
- **Safe locations:** Offline cache of shelters, hospitals, and water points with last-verified timestamps.
- **Help requests:** Geotagged, prioritized by a transparent score, and traceable through a hash-chained status log.
- **Family check-ins:** End-to-end encrypted safe, need help, and not yet checked states.
- **Emergency guidance:** Offline content packs by hazard type.

## Priority score

```text
Priority = Severity × 0.4 + Vulnerability × 0.25 + Time waiting × 0.2 + Corroboration × 0.15
```

## Success metrics

| Metric | Target |
|---|---|
| Alert timeliness | Under 30 s online, under 2 min via SMS |
| Location accuracy | Accuracy radius shown on every request |
| Request traceability | Full history for every request |
| Usability | First-time request in under 60 s |

## Repository layout

- `docs/`: problem statement, architecture, pitch deck, poster, demo video links
- `src/`: application code
- `demo/`: sample CAP alert feed for testing
- `tests/`: unit and integration tests

## Demo materials

- Pitch deck: `docs/presentation/Beacon-Pitch.pptx`
- Poster: `docs/poster/beacon-poster.svg`
- Demo video: [add YouTube or Google Drive link here]

## Running locally

```bash
# Add setup steps once src/ is implemented
```

## Simulated components

- SMS gateway: mocked for the demo
- BLE peer relay: simulated with a second browser tab

## Team

See [team/CONTRIBUTORS.md](team/CONTRIBUTORS.md).

## License

[Choose a license, e.g., MIT]
