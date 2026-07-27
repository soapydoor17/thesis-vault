---
title: "Small Satellite Orbit Determination Methods Based on the Doppler Measurements by Belarusian State University Ground Station"
year: 2021
authors: Alexander A. Spiridonov, Vladimir A. Saetchnikov, Dmitrii V. Ushakov, Vladimir E. Cherny, Alexey G. Kezik
tags: 
  - zotero 
---

[1]  A. A. Spiridonov, V. A. Saetchnikov, D. V. Ushakov, V. E. Cherny, and A. G. Kezik, ‘Small Satellite Orbit Determination Methods Based on the Doppler Measurements by Belarusian State University Ground Station’, _IEEE J. Miniat. Air Space Syst._, vol. 2, no. 2, pp. 59–66, Jun. 2021, doi: [10.1109/JMASS.2020.3047456](https://doi.org/10.1109/JMASS.2020.3047456).

- zotero: [zotero://select/library/items/HVCXTAZ4](zotero://select/library/items/HVCXTAZ4)
- url: https://ieeexplore.ieee.org/document/9309033/
- pdf: [PDF](file:///home/tobey/Zotero/storage/9QC37TN3/Spiridonov%20et%20al.%20-%202021%20-%20Small%20Satellite%20Orbit%20Determination%20Methods%20Based%20on%20the%20Doppler%20Measurements%20by%20Belarusian%20State%20Un.pdf)

# Abstract
The Doppler measurements of the small satellites (SSs) were carried out using the Belarusian State University ground station. Measurements of a telemetry signal for the several satellite orbits with a limited number of data on one pass were performed. Two methods for orbit determination of an SS are considered. The ﬁrst method is based on the perturbed circular motion prediction model for the satellite orbit with experimental data from radio signal processing. It does not require additional information from the NORAD database of satellite orbital parameters. Using predicted data of the tracking angles of antenna systems and the Doppler frequency shift of the telemetry radio signal, the ground station of the Belarusian State University received and successfully decoded telemetry packets from unknown SS LUOJIA-1 01. The second method is based on the SGP4 model and requires additional information from the NORAD two-line element (TLE) catalog of the satellite orbital parameters. An unknown SS LUOJIA-1 01 was identiﬁed using the NORAD TLE catalog based on a probabilistic estimation of the elevation angle and the Doppler frequency shift of receiving telemetry signals.

# Highlights



# Notes



%% Import Date: 2026-06-21T17:14:33.395+08:00 %%

---
# Research Question
Can a university ground station determine or correct the orbit of a small satellite (CubeSat/nanosatellite) using only Doppler frequency shift measurements from a single satellite pass, without additional data sources? And, separately, can unknown satellites be identified from the NORAD TLE database using only those same measurements?
# Methods
### Ground station setup
- 435–438 MHz Yagi-Uda antenna with circular polarisation
- Software-defined radio (SDR) receiver + YAESU G-5500 azimuth-elevation rotator
- GPS receiver for precise time synchronisation (1PPS signal)
- Digital oscilloscope to record exact signal arrival times and frequencies

### Method A — perturbed circular motion (no TLE required)
- Models the satellite orbit using four parameters: orbital period T, inclination i, latitude argument u, and longitude of ascending node Ω
- Accounts for Earth's non-uniform gravity (J2 zonal harmonic [[(Second) Zonal Harmonics]]) causing slow drift in Ω and u over time
- For each candidate set of orbital parameters, numerically simulates the expected elevation angle and Doppler frequency shift during the pass
- Compares simulated values to measured values; a 'probability of success' score filters good matches from bad ones
- Does not require a prior TLE — works from the Doppler curve shape alone

### Method B — NORAD TLE database search
- Uses observed signal timing intervals and average received frequency to narrow the search to plausible orbital periods and inclinations
- Runs SGP4 propagation on candidate satellites from the NORAD catalog to predict their Doppler curves
- Probabilistically ranks candidates by how well their predicted elevation and Doppler match the measured data
- Identifies the unknown satellite by finding the highest-ranked match

# Key Findings
- Method A successfully determined the orbital parameters of a known nanosatellite (CubeBel-1) using 25 measurements from a single pass, with elevation/azimuth prediction errors under 3° and Doppler prediction error under 160 Hz.
- Method B successfully identified an unknown satellite transmitting in the 435–445 MHz band as LUOJIA-1 01 (NORAD #43485) — confirmed against the NORAD catalog — using only Doppler and timing data from several passes.
- Adding the Doppler frequency shift criterion (Method B, β₂) dramatically reduced ambiguity: from 55,259 plausible orbital parameter sets down to just one with 96% confidence.
- Elevation-only matching (β₁) leaves hundreds or thousands of candidate orbits; Doppler matching (β₂) narrows this to single digits in practice.
- A cooperative network of a stationary university station plus mobile stations with omnidirectional antennas could further improve coverage and accuracy.

# Limitations
- Single ground station only — geometry is inherently weak. A satellite passes overhead for only 5–12 minutes, and all measurements come from one viewing angle.
- Only range-rate (Doppler) is measured — no range, azimuth, or elevation data from the radio. This limits how well the full 6-parameter orbit state can be determined from a single pass.
- Method A is designed for near-circular orbits (low eccentricity). It would need modification for highly elliptical orbits.
- The method requires the satellite to be transmitting on a known or discoverable frequency — silent satellites cannot be tracked this way.
- Pointing the directional Yagi antenna requires a reasonably accurate prior prediction; a poor TLE makes acquisition difficult.
- Results validated on only two satellites (nSight-1 and CubeBel-1); broader validation across different orbit types would strengthen confidence.

# Relevance
- Demonstrates that single-station Doppler-only orbit determination is feasible at university scale with off-the-shelf hardware
- Method A is a more autonomous approach than what Binar currently does (TLE correction only) — could be a direction for the thesis.
- Method B is relevant to space domain awareness (tracking unknown or uncooperative objects)
- The paper highlights the directional antenna pointing problem explicitly, motivating either the omnidirectional antenna direction or the SatNOGS multi-station direction.
- The 'probability of success' framework is a useful way to quantify how much information different measurement types (elevation vs Doppler) contribute -> could inform how to frame a SatNOGS or ranging study.
- Reference for the SGP4/TLE prediction pipeline that any orbit determination approach for Binar will need to interface with

# Questions and Follow-Up

New Terms
[[Telemetry]], [[Doppler Frequency Shift]], [[Perturbed Circular Motion Model]], [[Two-Line Element (TLE)]], [[Azimuth]], [[Keplerian Motion Model]], [[(Second) Zonal Harmonics]], [[Slant Range]], [[Range Rate]], [[Secular Perturbation]]