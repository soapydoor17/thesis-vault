# Functionality Requirements
**What does the solution need to do?**
1. The tool shall process time-tagged SatNOGS frequency observations to get the Doppler shift relative to the centre frequency of the carrier.
2. For a single ground station, the tool shall run differential correction (DC) to correct an *a priori* orbit (prior TLE) using the range-rate data from at least 2 passes, and shall report the solution's uncertainty
3. The tool shall detect and report an ill-conditioned or unobservable solution instead of returning an orbit
4. The tool should run initial orbit determination (IOD) from Doppler observations without a prior TLE.
5. For multiple ground stations, or additional beacon information beyond Doppler, the tool should investigate whether DC converges reliably to the true orbit from a wide range of initial guesses, without requiring a good a priori orbit. Where this is confirmed, the tool should support DC without a supplied a priori.

# Performance Requirements
**How well does it need to perform? Include metrics**
1. For DC, the tool shall produce a solution within 30 seconds on a standard laptop once the data is available.
2. On held-out passes within 24 hour of the fit, the tool's Doppler prediction RMS error shall be within 10% of the RMS noise of the input observation, and lower than the TLE-predicted Doppler on the same passes
3. The tool shall converge from a TLE that is at most 8 hours older than the first observation
4. The tool shall predict the Doppler shift for all passes in the next 24 hours

# Interface Requirements
**How will it interact with other systems and people?**
1. The orbit determination algorithm shall take in the following inputs:
	- Satellite ID
	- Doppler shift measurement of the CubeSat from SatNOGS
	- Central frequency of the carrier
	- Ground station geodetic coordinates (lat / lon / alt)
	- Date-time stamp
	- Prior TLEs (for DC)
2. The orbit determination algorithm shall give the user the following outputs:
	- Classical Orbital Elements
		- $a$ - semi-major axis (m)
		- $e$ - eccentricity
		- $i$ - inclination (deg)
		- $\Omega$ - RAAN (deg)
		- $\omega$ - Argument of perigee (deg)
		- $M_0$ - Initial mean anomaly (deg)
	- Perturbation Coefficients
		- Second derivative of mean motion
		- B* drag term
	- Residuals
	- Uncertainty
3. The tool shall be open-source, written in Python, and documented with a worked example

# OUT OF SCOPE
- Spacecrafts with manoeuvres and non-LEO orbits - this project is focusing on university team CubeSats in low earth orbits
- GPS-derived orbits - Binar is trying to do OD with minimal information, allowing them use the CubeSat's limited space for other tasks.


---
# Notes
## Scope
Orbit determination of a LEO CubeSat with range-rate only (Doppler shift measurements)
- Initial orbit determination
- Differential correction for post-pass processing

Single station
Multi station with SatNOGS as an extension

Not real time as the observations will be received post-pass. However want it to be near-real time

OUT OF SCOPE LIST:
- ???

## Target Accuracy
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

## User Requirements
Document who uses the tool (amateur ground stations, students, researchers), required inputs/outputs, and constraints (real-time vs. post-pass processing).

### Who would use the tool
- Binar
- Students
- Groundstation operators?

### Potential Questions
- What do you do when the TLE is off?
- How far off is too far off / What is the tolerance of each parameter?

### I/O
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

### Constraints
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