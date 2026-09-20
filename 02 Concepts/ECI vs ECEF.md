---
tags:
  - definition
---
## Definition

Both are 3D coordinate frames used to describe positions in space, but they differ in whether they rotate with the Earth.

**ECI — Earth-Centered Inertial**
- Origin at Earth's centre, axes fixed relative to the distant stars (non-rotating).
- Used for describing satellite orbits, because Kepler's laws and orbital mechanics equations assume you're working in a frame that isn't rotating — a satellite in orbit traces a clean ellipse in ECI coordinates.
- Orbital elements ($a, e, i, \Omega, \omega, M_0$) and propagated $\textbf r, \textbf v$  all live in this frame.

**ECEF — Earth-Centered, Earth-Fixed**
- Origin at Earth's centre too, but the axes rotate _with_ the Earth (so a fixed point on the ground, such as the ground station, has constant ECEF coordinates).
- This is the natural frame for describing a ground station's location, since lat/lon/altitude naturally map to ECEF

