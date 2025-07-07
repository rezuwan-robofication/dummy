
# 1 Scope & Objective
Specify the mandatory features, constraints, and lifecycle obligations for a **3D Additive Manufacturing System (AMS)** facilitating rapid prototyping and production of three-dimensional artifacts via successive layer deposition. This system shall encompass capabilities for digital file ingestion, material extrusion, thermal management, and comprehensive process monitoring to ensure geometric fidelity and material integrity.

# 2 Normative References
(No normative references provided in the original document)

# 3 Definitions & Abbreviations
* [cite_start]**AMS** – Additive Manufacturing System [cite: 1]
* **G-code** – Geometric code (standard language for controlling CNC machines, including 3D printers)
* **HMI** – Human-Machine Interface
* [cite_start]**STL** – Stereolithography (common file format for 3D models) [cite: 5]
* [cite_start]**OBJ** – Object (another common file format for 3D models) [cite: 5]
* [cite_start]**FDM** – Fused Deposition Modeling (common 3D printing technology) [cite: 27]
* [cite_start]**SLA** – Stereolithography Apparatus (a type of 3D printing technology) [cite: 27]
* [cite_start]**PLA** – Polylactic Acid [cite: 20]
* [cite_start]**ABS** – Acrylonitrile Butadiene Styrene [cite: 21]
* [cite_start]**PETG** – Polyethylene Terephthalate Glycol-modified [cite: 22]
* [cite_start]**TPU** – Thermoplastic Polyurethane [cite: 23]
* [cite_start]**CAD** – Computer-Aided Design [cite: 15]
* **PID Control** – Proportional-Integral-Derivative control (feedback loop mechanism)
* [cite_start]**Thermistors/Thermocouples** – Temperature sensing devices [cite: 55, 74]
* [cite_start]**Non-manifold edges** – Edges shared by more than two faces in a 3D model [cite: 11]
* [cite_start]**Intersecting triangles** – Overlapping triangles in a 3D model mesh [cite: 11]
* [cite_start]**Build volume** – The physical confines of the print bed [cite: 12]
* [cite_start]**Overhangs** – Sections of a 3D print that extend outwards without direct support from below [cite: 13]
* [cite_start]**Layer height** – The vertical resolution of a 3D print [cite: 64]
* [cite_start]**Infill pattern/density** – How much material is printed within the object's interior [cite: 69]

# 4 High-Level Architecture Context
The AMS comprises a control plane, a motion control subsystem, an extrusion subsystem, a thermal management subsystem, and a sensor array, all orchestrated by an embedded processing unit and interfaced via a host HMI.

**Interfaces**

* **Data Ingress** – `USB 2.0/3.0` for local file transfer; `Ethernet (TCP/IP)` for network-based print job submission.
* **Actuator Control** – Stepper motor drivers (e.g., `DRV8825`) for X/Y/Z axes; `PWM` for heater elements.
* [cite_start]**Sensor Suite** – Thermistors/RTDs [cite: 74] for thermal monitoring; Optical/inductive end-stops; [cite_start]Capacitive/piezoelectric bed leveling probes[cite: 37]; [cite_start]Encoder for filament runout detection[cite: 77].

# 5 System Requirements
Numbering convention: `3DP-⟨Group⟩-###`; **“shall” = mandatory**.

## 5.1 Functional («Functional»)
| ID | Requirement |
| --- | --- |
| 3DP‑FUNC‑001 | [cite_start]System shall accept and validate print job files (e.g., `.gcode`, `.stl`, `.obj`) upon upload via specified data ingress interfaces, executing integrity, completeness, and printability checks[cite: 4, 5, 8, 9, 12]. [cite_start]This validation shall identify non-manifold edges and intersecting triangles [cite: 11][cite_start], and confirm adherence to the defined build volume[cite: 12]. |
| 3DP‑FUNC‑002 | [cite_start]System shall provide a user interface for selection of compatible print materials (e.g., PLA, ABS, PETG, TPU, nylon, polycarbonate, composites) [cite: 19, 20, 21, 22, 23][cite_start], influencing material-specific thermal profiles and deposition parameters[cite: 24, 25]. |
| 3DP‑FUNC‑003 | [cite_start]System shall execute an automated or user-assisted bed leveling routine (e.g., manual, automatic with proximity sensors, BLTouch, strain gauges) [cite: 30, 33, 34, 36, 37][cite_start], ensuring planar compensation across the build platform prior to print initiation[cite: 38, 39]. |
| 3DP‑FUNC‑004 | [cite_start]System shall implement pre-print thermal conditioning, achieving and stabilizing target temperatures for the print bed and extrusion nozzle using thermistors/thermocouples [cite: 43, 44, 49, 55][cite_start], based on selected material properties (e.g., PLA 50-60°C bed, 180-220°C nozzle; ABS 90-110°C bed, 230-260°C nozzle)[cite: 48, 54]. |
| 3DP‑FUNC‑005 | [cite_start]System shall detect and classify operational anomalies (e.g., thermal runaway, filament discontinuity, clogged nozzle, layer shift, power outage) [cite: 76, 77, 78, 88] [cite_start]and trigger an immediate print pause with concurrent user notification via HMI[cite: 85, 89]. |
| 3DP‑FUNC‑006 | [cite_start]System shall enable resumption of an interrupted print job subsequent to anomaly remediation and user confirmation[cite: 93]. [cite_start]The system shall recall the last print position and thermal state[cite: 95]. |
| 3DP‑FUNC‑007 | [cite_start]System shall initiate a controlled cool-down sequence for the print bed upon completion of the print job [cite: 100, 105][cite_start], ensuring safe object removal conditions by leveraging material contraction[cite: 102]. |
| 3DP‑FUNC‑008 | [cite_start]System shall generate and archive a comprehensive print log encompassing operational status (completed, aborted, failed) [cite: 113, 114][cite_start], material consumption (meters/grams) [cite: 118][cite_start], and print statistics (total print time, max temperatures, power consumption, manual interventions) [cite: 117, 119] [cite_start]post-completion[cite: 111, 116]. |

## 5.2 Performance («Performance»)
(No specific performance requirements provided in the original document)

## 5.3 Safety («SafetyGoals»)
| ID | Requirement |
| --- | --- |
| 3DP‑SAFE‑030 | [cite_start]System shall implement thermal runaway protection, detecting uncontrolled temperature increases and initiating an emergency shutdown to prevent overheating and fire hazards[cite: 88]. |
| 3DP‑SAFE‑031 | System shall ensure that electrical components are isolated and grounded to prevent electrical shock hazards. |
| 3DP‑SAFE‑032 | System shall incorporate physical safeguards (e.g., enclosures, interlocks) to prevent user contact with heated or moving components during operation. |

## 5.4 Cyber-Security
(No specific cyber-security requirements provided in the original document)

## 5.5 Diagnostics & Logging
| ID | Requirement |
| --- | --- |
| 3DP‑DIAG‑050 | [cite_start]System shall continuously monitor and log nozzle and bed temperatures via a closed-loop PID control system using thermistors/thermocouples[cite: 55, 74, 75], recording deviations from target values. |
| 3DP‑DIAG‑051 | [cite_start]System shall monitor and log filament volumetric flow rate, detecting extrusion anomalies (e.g., clogging, runout) via dedicated sensors or extruder motor feedback[cite: 77, 78]. |
| 3DP‑DIAG‑052 | [cite_start]System shall monitor and log real-time nozzle Cartesian coordinates (X, Y, Z) via stepper motor encoders [cite: 79] [cite_start]and detect deviations from planned toolpaths, logging any skipped steps or collisions[cite: 80, 81]. |
| 3DP‑DIAG‑053 | [cite_start]System shall log all user interventions, including print pauses, resumptions, and manual parameter adjustments[cite: 119]. |

## 5.6 Reliability & Availability
(No specific reliability or availability requirements provided in the original document)

## 5.7 Human-Machine Interface
| ID | Requirement |
| --- | --- |
| 3DP‑HMI‑070 | [cite_start]System shall provide clear, real-time status updates on print progress, including current layer, estimated time remaining, and current temperatures[cite: 116, 117]. |
| 3DP‑HMI‑071 | [cite_start]System shall provide intuitive controls for print initiation, pausing, and cancellation[cite: 85, 93]. |
| 3DP‑HMI‑072 | [cite_start]System shall display clear, actionable error messages with suggested remediation steps upon anomaly detection[cite: 89, 90]. |

## 5.8 Sustainability
(No specific sustainability requirements provided in the original document)

# 6 Verification & Validation Strategy
* **Software-in-the-Loop (SiL) Testing**: Simulation of slicer logic and G-code parsing to verify print path generation and parameter application.
* **Hardware-in-the-Loop (HiL) Testing**: Integration of embedded controller with simulated mechanical and thermal plant models to validate real-time control, sensor feedback, and error handling.
* **Functional Prototype Testing**: Validation of full print process from file upload to object removal, assessing print quality, dimensional accuracy, and adherence to material properties.
* **Long-Term Reliability Testing**: Extended print cycles to stress test components and identify mean time between failures (MTBF).

# 7 Traceability & Change Control
All `3DP-xxx-###` requirements shall be managed as «requirement» elements within a Requirements Management System. Each requirement shall possess bidirectional traceability links to design artifacts («satisfy»), test cases («verify»), and lower-level component specifications («deriveReqt»). Changes shall follow a formal change control board (CCB) process.
