
# To Do
- [ ] Turn requirements into testable statements

# Scope
Orbit determination of a LEO CubeSat with range-rate only (Doppler shift measurements)
- Initial orbit determination
- Differential correction for post-pass processing

Single station
Multi station with SatNOGS as an extension

Not real time as the observations will be received post-pass. However want it to be near-real time

OUT OF SCOPE LIST:
- ???

# Target Accuracy
Specify quantitative accuracy targets (e.g. position/velocity error, Doppler-frequency error in Hz) benchmarked against SGP4/TLE performance in the literature.

Will depend on pass count and spacing (N passes over t hours to achieve X)

USABILITY - 

Early post-deployment
- Give measurement before TLE is available

Later passes
- Predict Doppler curve better than TLE does (need metric for this)
	- Fit with passes 1 to N then predict pass N+1 and compare against TLE

[[Spiridonov - 2021 - Small Satellite Orbit Determination Methods]]
- Uses real data, but doesn't explicitly show state errors, only shows error of Doppler shift and pointing angle
[[Coyle - 2001 - Orbit Determination at a Single]]
- Shows state error, but doesn't use real data (only simulated)

-> THUS could possibly use real data and show demonstrate state error 

Measuring state error -> easiest is using simulated data. Could use TLE as 'truth', but it would have its own errors. A satellite with GPS would be BEST, but probably not possible?

-> Analyse Doppler with real data, and state error with simulated data?

# User Requirements
Document who uses the tool (amateur ground stations, students, researchers), required inputs/outputs, and constraints (real-time vs. post-pass processing).

## Who would use the tool
- Binar
- Students
- Groundstation operators?

### Potential Questions
- What do you do when the TLE is off?
- How far off is too far off / What is the tolerance of each parameter?

## I/O
**Inputs:**
- Satellite ID
- Doppler shift measurement of the CubeSat from SatNOGS
- Central frequency 
- Unknown transmitter offset
- Ground station lon/lat/att
- Time stamp
- Prior TLEs (optional)

**Outputs:**
- 6 'Corrected' Orbital Elements
	- $a$ - semi-major axis (m)
	- $e$ - eccentricity
	- $i$ - inclination (deg)
	- $\Omega$ - RAAN (deg)
	- $\omega$ - Argument of perigee (deg)
	- $M_0$ - Initial mean anomaly (deg)
- 2 Perturbation Coefficients
	- Mentioned last meeting - what are these?

## Constraints
- SatNOGS API limits
	- How accurate are timestamps?
	- Assuming satellite moves at about 7km/s and $f_c$ =438MHz
		- ${}\frac{438\times 10^6}{c}\times 7000 \approx 10\text{ kHz}{}$ error per second off
	- How long does it take for data to become available?
		- -> Estimate with x minutes of data being available? (Latency requirement)
	- What information does SatNOGS give?
- Noisy / missing frequency data
	- [[Spiridonov - 2021 - Small Satellite Orbit Determination Methods]] mentions 200Hz allowance
- Python / OS Support
- Antenna
	- At Binar GS - 430-450MHz (limited to UHF satellites)
	- Multi station -> Per station hardware limitations and noise parameters