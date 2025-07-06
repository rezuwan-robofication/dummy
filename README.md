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
Safety lead approva
