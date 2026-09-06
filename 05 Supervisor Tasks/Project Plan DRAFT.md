---
Assigned: 2026-07-28
Due:
Status: In-progress
Type: Admin
tags:
  - task
---

## Outline
Improved satellite tracking methods for navigation and space domain awareness with limited observations

Improved orbit determination of a satellite, through the use of the SatNOGS network.

Goal is a validated, documented and openly usable software tool that any university CubeSat team can use for their own missions. It would be great to improve TLE-only tracking, particularly in the early post-deployment period where official orbital parameters are the least accurate.

## Timeline of major milestones

[**Google Drive**](https://docs.google.com/spreadsheets/d/18umo9mk_7QiEGjc-JUyRol3QYPkEhNEB/edit?usp=sharing&ouid=112608109564620790408&rtpof=true&sd=true)

**Duration:** Aug 2026 – Jun 2027 (11 months) **Phases:** Requirements → Algorithm Design → SatNOGS Integration → Validation → Packaging & Documentation

| Due Date         | Phase               | Milestone                            | Description / Deliverable                                                                                                                                       |
| ---------------- | ------------------- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **July and Aug** | Requirements        | Project kickoff                      | Confirm scope, supervisor sign-off, set up project tracker, and reference library (Doppler/TLE OD literature).                                                  |
| **Sept 11**      | Algorithm Design    | OD algorithm selected                | Choose and justify the orbit determination method (e.g. Doppler-shift least-squares, circular perturbed motion model) with trade-off analysis.                  |
| **Sep 15**       | Requirements        | Target accuracy defined              | Specify quantitative accuracy targets (e.g. position/velocity error, Doppler-frequency error in Hz) benchmarked against SGP4/TLE performance in the literature. |
| **Sep 22**       | Requirements        | User requirements finalized          | Document who uses the tool (amateur ground stations, students, researchers), required inputs/outputs, and constraints (real-time vs. post-pass processing).     |
| **Oct 13**       | Requirements        | Requirements spec sign-off           | Formal requirements document approved by supervisor.                                                                                                            |
| **Oct 20**       | Algorithm Design    | Software architecture draft          | High-level architecture: data ingestion, OD engine, output/reporting modules; define interfaces between them.                                                   |
| **Oct 26**       | —                   | **Progress Report Due**              | Submit formal progress report covering requirements + algorithm approach.                                                                                       |
| **Nov 28**       | Algorithm Design    | Algorithm prototype complete         | Core OD algorithm implemented and unit-tested on simulated/synthetic Doppler data.                                                                              |
| **Dec 12**       | SatNOGS Integration | SatNOGS API/data access working      | Pull real observation data (frequency/time-tagged telemetry) from SatNOGS for a test satellite.                                                                 |
| **Dec 19**       | SatNOGS Integration | Data ingestion pipeline              | Parser/pre-processor converts raw SatNOGS observations into the format the OD engine expects; handle missing/noisy data.                                        |
| **Jan 16**       | SatNOGS Integration | End-to-end pipeline demo             | SatNOGS data flows through ingestion → OD engine → orbit estimate, on at least one real pass.                                                                   |
| **Jan 30**       | SatNOGS Integration | Integration testing complete         | Pipeline run against multiple satellites/passes; bugs and edge cases (dropouts, multiple passes) resolved — closes Phase 3.                                     |
| **Feb 13**       | Validation          | TLE benchmark dataset assembled      | Collect published TLEs for test satellites over the same time windows as your observations.                                                                     |
| **Feb 27**       | Validation          | Initial accuracy results             | Compare estimated orbits against published TLEs; compute error metrics (position, velocity, Doppler residuals).                                                 |
| **Mar 13**       | Validation          | Algorithm refinement                 | Tune/adjust algorithm based on validation results; re-run comparison.                                                                                           |
| **Mar 27**       | Validation          | Validation report complete           | Finalized accuracy assessment vs. TLEs, with discussion of error sources — closes Phase 4. Good checkpoint for a supervisor progress meeting.                   |
| **Apr 10**       | Packaging & Docs    | Tool packaging                       | Package as installable CLI/library; config files, sample data included.                                                                                         |
| **Apr 24**       | Packaging & Docs    | User documentation draft             | Installation guide, usage examples, API/CLI reference.                                                                                                          |
| **May 8**        | Packaging & Docs    | Thesis draft — technical chapters    | Requirements, methodology, algorithm design, and implementation chapters drafted.                                                                               |
| **May 22**       | Packaging & Docs    | Thesis draft — results & full review | Validation results and discussion chapters drafted; full draft sent to supervisor for review.                                                                   |
| **Jun 5**        | Packaging & Docs    | Revisions complete                   | Incorporate supervisor feedback; final proofread; documentation finalized.                                                                                      |
| **Jun 26**       | —                   | **Final Submission**                 | Submit final thesis + packaged tool + documentation.                                                                                                            |

## Notes

- **Buffer built in:** each phase has ~1–2 weeks of slack before its "closes phase" milestone — useful if SatNOGS data access or algorithm tuning takes longer than expected (common bottlenecks in this kind of project).
- **Two natural check-in points** beyond the required Oct progress report: end of Phase 3 (late Jan) and end of Phase 4 (late Mar)
- **Validation phase is the highest-risk phase** — TLE accuracy itself varies, so budget real time for comparing against multiple satellites/passes, not just one.

## Major project challenges
Including procurement, manufacturing, admin, etc.

Getting SatNOGS access
- University IT security may need to approve outbound API 
- Getting data from SatNOGs and getting it in the correct format

Figuring out algorithm

## Statement of research output
- A working and documented orbit determination software tool that takes in SatNOGS network observations and outputs orbital state estimates for target satellites
- A quantified accuracy assessment of the satellite against known TLEs and possibly against in-house CubeSat mission

## Why is this project useful?
One of the main current orbit determination is going through the [[NORAD]] database, which is an satellite orbit catalog operated by the US Space Force. The developed tool from this project would allow people to get readings from satellites quicker than fetching the TLE sets from the NORAD database. Users will be able to get readings from their own ground stations ASAP, instead of waiting for the next reading from the US government. Further, this tool will allow users to information on a satellite's orbit more independently, and without being reliant on a large government organisation. 
Also NORAD can be irregular

[Propagation of CubeSats in LEO using NORAD Two Line Element Sets: Accuracy and Update Frequency](https://arc.aiaa.org/doi/10.2514/6.2013-4944)
- TLEs are not accompanied by any indicators of accuracy or consistency
- Thus independently-calculated orbit estimates have value even when TLE is available


CASE STUDY ON CuPID: https://arxiv.org/pdf/2304.04702
- ANOMALY TESTING AND RESULTS - EARLY TLE'S: Multiple sources (including NORAD) all supplied multiple TLEs post-deployment.  This was because not all satellites in the region had been claimed, meaning multiple (six) objects had to be tracked
- IMPROVEMENTS mentions that if the satellite had beaconing, they would have been able to use SatNOGS to get nearly continuous monitoring and would have been able to locate the satellite in its early operations by the TLE's strength

https://digitalcommons.usu.edu/cgi/viewcontent.cgi?article=3222&context=smallsat&httpsredir=1
- Prior TLE assessments estimated to have 1km of error
- Error deployed CubeSats is a lot higher than prior assessments
- To improve error, batch least squares estimate used to estimate current based on prior TLE
- Resulted in up to 95% reduction 

For a single university CubeSat team in the first weeks after deployment, a self-run Doppler OD pipeline can give a faster, more accurate, and more trustworthy fix than waiting on TLEs, without depending on a government tasking queue.