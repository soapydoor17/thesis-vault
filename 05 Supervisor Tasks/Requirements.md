# Functionality Requirements
**What does the solution need to do?**
1. The tool shall process time-tagged SatNOGS frequency observations to get the Doppler shift relative to the centre frequency of the carrier.
2. The tool shall run initial orbit determination (IOD) from Doppler observations without a prior TLE, given ????
	- Coyle assumes launch provider's is chill
	- Spiridonov does grid search? 
	- More research here probs
	- <span style="color:red">What is IOD input?</span> 
3. The tool shall run differential correction (DC) to correct an *a priori* orbit (IOD output or prior TLE) using the range-rate data from at least 2? observations, and shall report the solution's uncertainty
4. The tool shall detect and report an ill-conditioned or unobservable solution instead of returning an orbit

# Performance Requirements
**How well does it need to perform? Include metrics**
1. The tool shall produce a solution within 5 mins on a standard laptop once the data is available.
	- <span style="color:red">Is this realistic for this type of problem?</span>
2. On held-out passes within [24 h] of the fit, the tool's Doppler prediction RMS error shall be at most [200 Hz] and lower than the TLE-predicted Doppler on the same passes
	- BSU uses 200Hz as their tolerance
3. Given 2 passes over a 12 hour period with noise of ${}\sigma{}$ Hz, the tool shall estimate position at epoch to within 10 km
4. The tool shall converge from a TLE that is at most 8 hours older than the first observation

# Interface Requirements
**How will it interact with other systems and people?**
1. The orbit determination algorithm shall take in the following inputs:
	- Satellite ID
	- Doppler shift measurement of the CubeSat from SatNOGS
	- Central frequency of the carrier
	- Unknown transmitter offset
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
		- TLE has the first and second derivative of mean motion, as well as B* drag term
		- <span style="color:red">WHAT PERTURBATION COEFFICIENTS SHOULD IT PREDICT?</span> 
3. The tool shall predict the Doppler shift for the next **X** passes
	- <span style="color:red">WHAT IS X?</span>
4. The tool shall be open-source, written in Python, and documented with a worked example

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