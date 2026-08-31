---
tags:
  - definition
---
## Definition
Orbit catalog where every object gets a NORAD Catalog Number (SATCAT ID) and a periodically updated TLE ([[Two-Line Element (TLE)]]) set. Some objects get updates daily, others go weeks without updates (especially right after launch).

NOT raw observations.
### Challenges
- Updates can be inconsistent - some objects get daily updates, others go a while between updates.
- Particularly struggles right after deployment
- Recent problem - ran out of 5-digit IDs. Many libraries that handle TLE parsing aren't yet able to handle this
- No control/visibility into raw tracking data - only get fitted orbital elements, no underlying observations or Doppler/frequency data

## Source
[KeepTrack](https://keeptrack.space/space-terms/satellite-catalog)
