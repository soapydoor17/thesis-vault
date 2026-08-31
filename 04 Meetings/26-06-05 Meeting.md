#meeting

# Agenda
Papers
Angle of project - hardware vs range data vs SATNOG
What will the next year look like?

## Paper 1 - Capability BINAR would like
[[Spiridonov - 2021 - Small Satellite Orbit Determination Methods]]
Method A - Perturbed circular motion
- Predicts what the Doppler curve should look like for any orbital state
- Then searches for orbital parameters that make the predicted curve match the true curve
- Doesn't require prior TLE to get started

Method B - NORAD TLE search
- Use measurements to probabilistic search through NORAD TLE to find which satellite best matches the elevation profile and Doppler shift pattern
- Figures out which satellite it is from signal characteristics

TAKEAWAY - successfully received telemetry from satellites they'd never deliberately tracked, and identified unknown satellites using only a single ground station and Doppler measurements.

## Paper 2 - Currently BINAR Orbit Determination
[[Coyle - 2001 - Orbit Determination at a Single]]
Given imperfect initial guess of the satellite's orbit (from the TLE, or from the launch provider), can it be improved using only Doppler shift measurements from a single ground station?

Differential correction with batch least squares
1. Start with your best initial orbit estimate (position and velocity at a reference time).
2. Propagate that estimate forward to each time you took a Doppler measurement, and compute what the Doppler _should have been_.
3. Find the difference between what you predicted and what you actually measured (the "residual").
4. ***Mathematically work out how to adjust the initial orbit estimate to shrink those residuals***
5. Repeat until the residuals are small enough.

TAKEAWAY - can't get a fully independent orbit solution from range-rate-only data at one station (there are 6 unknowns but the Doppler curve doesn't give you enough information on its own to solve them all uniquely). However, you can significantly improve a poor initial estimate.

**Convergence improvement technique** (a modified Marquardt method) - the algorithm works even when the initial guess is quite far off (important as TLEs degrade over time and the initial error can be large)

## Approach Thoughts
### Hardware - Antenna
Binar currently has directional antenna, look into omni-directional antenna
- Pretty common, but could be interesting?
- Would this just be analysing different options already out in the market? Or like designing something specifically for Binar?

### Adding Range Data
Binar currently only uses range-rate data. Should be able to get range data too but not set up yet
- Would this be figuring out how to get the range data and then seeing how it improves the orbit determination?

### SatNOGS Integration
Global network of satellites, most with omni-directional antenna, all openly sharing data
- Create an algorithm for multi station orbit determination rather than single station?

### My thoughts
SatNOGS integration sounds like the most useful project out of these three.

# Notes
Look at if multi GS 
SatNog for orbit determination before
Other solutions for orbit determinations
Especially range rate only

Gradient Descient
- Louisville Marquis ?
vs
Stochastic Kalman filtering

PhD from Curtin on Orbit Determination - Trent ??? Kyle will send

# To Do
- [ ] 