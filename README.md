1. Scope & Objective
This system covers features and requirements for Advanced Driver-Assistance Systems (ADAS) and Automated Driving (AD) for SAE Levels 0 to 3. The system must:
Sense the environment using cameras, radar, and lidar.
Plan driving actions (like lane changes).
Control the vehicle’s steering, braking, and acceleration.

The goal is to reduce driver workload and prevent collisions.
2. Standards Referenced
These are the main standards the system must follow:
STD-001: ISO 26262-2:2018 – Automotive safety standard, used for assigning safety levels and documenting safety cases.
STD-002: ISO/SAE 21434:2021 – Cybersecurity standard, used to trace threat analysis results to requirements.
STD-003: UNECE R157 – Rules for lane-keeping systems; defines system limitations and driver handover triggers.
STD-004: ISO 23150:2023 – Ensures lidar sensor models are accurate enough for virtual testing.
3. Definitions
DDT: Dynamic Driving Task – all things the system must do to control the vehicle.
OC: Operational Conditions – road, weather, and speed conditions.
HMI: Human–Machine Interface – how the system communicates with the driver (e.g., screen, sounds).
4. System Architecture
The system has five main parts:
Perception – understands the environment (vehicles, people, etc.)
Localization – figures out where the car is.
Prediction – estimates where things (like cars or people) will go.
Planning – decides what the car should do next.
Actuation Interface – sends commands to the brakes, steering, etc.
Interfaces used:
Vehicle communication: Ethernet (100BASE-T1) and CAN-FD (2 Mbps).
Sensors: Front/rear cameras, radar at all corners, a roof-mounted lidar, and GNSS/IMU for location.
Driver monitoring: Camera inside the car and steering wheel touch sensor.
5. System Requirements
5.1 Functional Requirements
The system must support Lane Keep Assist (LKA) from 0–200 km/h on highways and major roads.
If the system is about to exit its operating limits (ODD), it must alert the driver at least 10 seconds in advance using sound and visual cues.
It must detect and classify vehicles, pedestrians, and cyclists up to 200 meters ahead with a delay of no more than 0.2 seconds.
In Level 2 or higher modes, it must control both steering and speed at the same time.
5.2 Performance Requirements
In clear weather, the system’s miss rate (false negatives) must be less than 0.0002 per frame; in light rain, less than 0.001.
Location error should be under 0.15 meters 95% of the time when driving at 120 km/h.
It must plan at least 5 seconds ahead and update plans every 100 milliseconds.
5.3 Safety Requirements
If there is a critical failure in planning, the car must go into a safe mode (brake fully, turn on hazard lights) within 1 second.
The system must detect a deviation in planned path (over 0.5 meters or 3 km/h) within 50 milliseconds.
If the driver looks away for more than 2 seconds, the system must issue a handover alert within 0.5 seconds.
5.4 Cybersecurity
All Ethernet messages between system components must be encrypted and authenticated using TLS or DTLS, with minimal delay (≤ 2 ms).
The system must check that software updates are signed and allow rollback only to previously signed versions.
5.5 Diagnostics & Logging
The system must record the last 30 seconds of sensor, location, and control data at 10 times per second, retrievable via standard diagnostic tools.
Fault codes must be set within 1 second of detection and only cleared after 3 fault-free drives.
5.6 Reliability & Availability
The system should work for at least 2,000 hours on average before a critical failure occurs.
If radar or lidar fails, the system should keep lateral control only (i.e., keep the lane).
5.7 Human–Machine Interface
Transition alerts should use a flashing amber icon (≥100 mm² and 200 cd/m² brightness) and a 1 kHz tone at 75 dB, blinking on/off every 500 ms.
The driver should be able to recognize the system mode (Off, Standby, Active, etc.) with a quick glance (≤ 2 seconds).
5.8 Sustainability
At least 75% of the mass of each sensor module should be made from recyclable materials, as documented under ISO 14021.
6. Verification & Validation
Testing Strategy:
Layer	Method	What’s Measured
MiL/SiL	Software simulations with pedestrian scenarios	Perception accuracy, planning speed
HiL	Inject faults in test hardware	Reaction time to failures
Track Tests	Real cars on test tracks	Handling system exit, driver takeover success
Fleet Testing	Logging data from 1 million km driven	Real-world safety (should meet 1×10⁻⁸ failures per hour)
 
7. Traceability & Change Control
Each requirement (AD-xxx-###) is marked as a requirement element in the model.
Use the following links:
«satisfy» to show which design elements meet a requirement.
«verify» to connect test cases with requirements.
«deriveReqt» when a lower-level requirement is derived from a high-level one.
All requirements are managed in a tool (SysModeler ALM) and go through a two-step review:
Architect review
Safety lead approval
 
1  Scope & Objective
Specify the mandatory features, constraints, and lifecycle obligations for an SAE Level 0–3 ADAS/AD domain that detects environment, plans maneuvers, and controls steering/brake/propulsion actuators to reduce driver workload and collisions.
2  Normative References
Ref	Title	Purpose in Model
STD-001	ISO 26262-2:2018 (“Automotive Functional Safety”)	ASIL assignment, safety case «artifact»
STD-002	ISO/SAE 21434:2021 (“Cyber-Security”)	TARA linking «threat» to reqts
STD-003	UNECE R157 (“Automated Lane Keeping Systems”)	Functional limits & transition demand
STD-004	ISO 23150:2023 (“Lidar Sensor Model Test”)	Sensor-model fidelity for virtual validation
 
3  Definitions & Abbreviations
DDT – Dynamic Driving Task.
OC – Operational Conditions (speed, weather, road class).
HMI – Human–Machine Interface (alerts, displays).
4  High-Level Architecture Context
The ADAS/AD domain comprises five logical subsystems (Perception, Localization, Prediction, Planning, Actuation-Interface) plus supporting ECUs and cloud services. Interfaces:
Vehicle Bus: 100BASE-T1 ETH / CAN-FD (2 Mbit s-¹).
Sensor Set: Front & rear cameras, 4 corner radar, 1 roof lidar, GNSS/IMU.
Driver Monitoring: In-cab camera + capacitive steering-wheel sensor.
5  System Requirements
Numbering convention: AD-⟨Group⟩-###; “shall” = mandatory.
5.1  Functional (map to SysML package «Functional»)
ID	Requirement
AD-FUNC-001	System shall provide Lane-Keep Assist (LKA) between 0 km h–¹ and 200 km h–¹ on road classes A (motorway) & B (multi-lane arterial).
AD-FUNC-002	System shall issue a Transition-Demand (TD) ≥ 10 s before Operating-Design-Domain (ODD) exit with visual+acoustic HMI.
AD-FUNC-003	Shall detect and classify vehicles, pedestrians, bicyclists within 200 m frontal range with ≤ 0.2 s latency.
AD-FUNC-004	In Level 2+ mode, **shall simultaneously control lateral and longitudinal path to keep
 
5.2  Performance (SysML package «Performance»)
ID	Requirement
AD-PERF-010	Mean perception false-negative rate ≤ 2 × 10⁻⁴ per frame in clear weather; ≤ 1 × 10⁻³ in moderate rain (2 mm h–¹).
AD-PERF-011	Localization RMS error ≤ 0.15 m (95 %) at 120 km h–¹ on 6-lane freeway.
AD-PERF-012	Planning horizon ≥ 5 s and replanning period ≤ 100 ms.
 
5.3  Safety (ISO 26262, SysML package «SafetyGoals»)
ID	Requirement
AD-SAFE-030	System shall achieve ASIL-D safe state (“minimal risk maneuver”: full brakes, hazard lights) within ≤ 1 s on critical path planner fault.
AD-SAFE-031	Dual-channel trajectory validity monitor shall detect lateral or longitudinal deviation > 0.5 m or > 3 km h–¹ within 50 ms.
AD-SAFE-032	Driver monitoring shall detect driver non-engagement (> 2 s eyes-off-road) and trigger TD within 0.5 s.
 
5.4  Cyber-Security
ID	Requirement
AD-CYB-040	All ECU-to-ECU Ethernet traffic shall be authenticated & encrypted (TLS-DTLS, AES-GCM-256) with ≤ 2 ms added latency.
AD-CYB-041	OTA update client shall verify image signature (ECC-384) and permit roll-back only to signed images.
 
5.5  Diagnostics & Logging
ID	Requirement
AD-DIAG-050	System shall store black-box logs (sensor raw, pose, controls) for last 30 s at ≥ 10 Hz, retrievable via UDS.
AD-DIAG-051	ISO 14229 DTCs shall be set within 1 s of fault detection and cleared only after 3 consecutive OK drive cycles.
 
5.6  Reliability & Availability
ID	Requirement
AD-REL-060	Mean Time Between Critical Failures (MTBCF) ≥ 2 000 h in mixed-traffic operation.
AD-REL-061	Graceful degradation strategy shall maintain LKA (lateral control only) if radar or lidar path is lost.
 
5.7  Human-Machine Interface
ID	Requirement
AD-HMI-070	TD visual alert shall use amber flashing icon ≥ 100 mm² luminance 200 cd m-², accompanied by 1 kHz tone 75 dB A, 500 ms on/off.
AD-HMI-071	System mode (Off, Standby, Active, TD, MRM) shall be identifiable by the driver within ≤ 2 s glance time.
 
5.8  Sustainability
ID	Requirement
AD-SUS-080	At least 75 % of sensor-module mass shall be recyclable materials documented by ISO 14021.
 
6  Verification & Validation Strategy
Layer	Method	Key Metrics
MiL/SiL	Closed-loop simulation with Euro NCAP VRU scenarios	Perception FN rate, planning latency
HiL	Fault injection on dual-channel safety monitors	Safe-state reaction time
Track Tests	UNECE R157 lanes & cut-in maneuvers	ODD exit handling, driver TD success
FOT	1 M km shadow-mode fleet logging	Residual risk rate vs KPI target 1 × 10⁻⁸/ h
 
7  Traceability & Change Control
Each AD-xxx-### requirement is a «requirement» element in your SysML model.
Use «satisfy» links from design blocks, «verify» links from test cases, and «deriveReqt» where lower-level ECU or sensor requirements emerge.
Managed in SysModeler ALM with two-step review (Architect → Safety Lead).
 
