# Problem Statement

Design a reliable, accessible disaster-response information system that:

- Delivers timely, verified alerts
- Identifies safe locations
- Collects and prioritizes geotagged requests for help
- Supports family safety check-ins
- Provides clear emergency guidance
- Protects personal data
- Works under low-connectivity conditions
- Presents actionable information to the public and responders

Success is measured by:

| Criterion | Meaning |
|---|---|
| Alert timeliness | Time from alert issue to display on the user's device |
| Location accuracy | Correctness of safe-location and request coordinates |
| Request traceability | Ability to audit every state change of a help request |
| Usability | How quickly and easily people complete key tasks |

## Context

During disasters, networks often fail first. People lose access to official warnings, shelter information, and ways to ask for help. Responders receive scattered, unverified reports and struggle to decide where to go first.

## Core tensions

- **Speed vs. trust:** Fast alerts can spread false information.
- **Openness vs. safety:** Anyone should be able to ask for help, but spam and fake requests must be limited.
- **Precision vs. privacy:** Accurate locations help responders, but location data is sensitive.
- **Features vs. connectivity:** Rich features must degrade to a minimal, offline-capable core.

## Assumptions

- Official alert sources publish Common Alerting Protocol (CAP) feeds.
- Users have some device with a browser, even if the network is unreliable.
- Responders have a login and a shared operational view.
- Feature phones may only support SMS.
