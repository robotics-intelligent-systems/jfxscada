# JFXSCADA — Open-Source AI Alternative Integration Architecture

## AI-Powered Supervisory Control, Industrial Data, Energy & Predictive Operations Platform

> **Repository:** `robotics-intelligent-systems/jfxscada`
>
> **Purpose:** reorganize the current JFXSCADA software compendium into a modular, standards-oriented, open-source-first architecture for SCADA/HMI, industrial protocols, telemetry, historian services, alarms, energy management, predictive maintenance, AI-assisted operations, digital twins and cross-domain integration.
>
> **Core principle:** JFXSCADA should not become one monolithic SCADA product. It should expose canonical asset, point, alarm, command, event, historian and model contracts so that HMI, protocol stacks, historians, analytics and AI engines can be replaced independently.
>
> **Safety principle:** AI agents, anomaly detectors and operator copilots may recommend or prepare actions, but they must not bypass PLC/SIS logic, deterministic interlocks, protection relays, emergency shutdowns, operator authorization or industrial safety procedures.

---

# 1. Source Project Direction

The current JFXSCADA README describes an:

> **AI-Powered Supervisory Control and Data Acquisition Platform**

and references a broad software and protocol ecosystem including:

- FUXA;
- AVEVA PI System;
- OpenRVDAS;
- TcOpen;
- Apache RocketMQ;
- Quasar;
- Matter;
- OpenMUC;
- jSML;
- j62056;
- jMBus;
- jEEBus SHIP;
- jEEBus SPINE;
- an open C implementation of EEBUS SHIP/SPINE;
- libiec61850;
- Java IEC 61850 MMS stack;
- j60870;
- jSCADA;
- OpenSCADA;
- Archimedes;
- VOLTTRON;
- OpenEMS;
- Rapid SCADA;
- TANGO;
- PyScada;
- SQLAlchemy;
- Scada-LTS;
- SCADA preprocessing for prognostics, health management and wind-turbine fault detection.

The repository also preserves:

```text
MBSE
├── CAD
├── CAM
└── CAS
```

The proposed architecture keeps the source compendium intact conceptually, but groups its components by capability and introduces a replaceable open-source AI/data plane.

---

# 2. Target Platform Vision

```text
FIELD DEVICES / PLC / IED / METERS
              ↓
      INDUSTRIAL PROTOCOLS
              ↓
       EDGE DATA GATEWAY
              ↓
      CANONICAL DATA MODEL
              ↓
         EVENT STREAM
              ↓
 ┌────────────┼─────────────┐
 ▼            ▼             ▼
SCADA/HMI  HISTORIAN   AI/ANALYTICS
 ▼            ▼             ▼
ALARMS     TRENDS      ANOMALIES
COMMANDS   EVENTS      FORECASTS
              │
              ▼
         DIGITAL TWIN
              │
              ▼
      OPERATOR DECISION
              │
              ▼
   POLICY / SAFETY GATEWAY
              │
              ▼
 PLC / CONTROL / FIELD ACTION
```

---

# 3. High-Level Alternative Architecture

```text
┌──────────────────────────────────────────────────────────────────────┐
│                           JFXSCADA                                   │
│ Assets | Tags | HMI | Alarms | Trends | Reports | Operations       │
└───────────────────────────────┬──────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────────┐
│                     INDUSTRIAL DATA FABRIC                           │
│ Canonical Model | Registry | Event Bus | Historian | Audit          │
└───────────────┬───────────────────┬──────────────────┬──────────────┘
                │                   │                  │
                ▼                   ▼                  ▼
        PROTOCOL GATEWAY        AI / PHM          DIGITAL TWIN
        OpenMUC / adapters     ML / RAG / RCA     Simulation
                │                   │                  │
                └───────────────────┼──────────────────┘
                                    ▼
┌──────────────────────────────────────────────────────────────────────┐
│                      POLICY / COMMAND GATEWAY                        │
│ RBAC | Approval | Interlocks | Command Validation | Audit           │
└───────────────────────────────┬──────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────────┐
│             PLC | RTU | IED | ENERGY | BUILDING | MACHINE           │
└──────────────────────────────────────────────────────────────────────┘
```

---

# 4. Architectural Layers

```text
Layer 1
Sensors / Actuators / PLC / RTU / IED

Layer 2
Industrial Protocols

Layer 3
Edge / Protocol Gateway

Layer 4
Canonical Industrial Data Model

Layer 5
Messaging / Event Streaming

Layer 6
SCADA / HMI / Alarm Management

Layer 7
Historian / Time-Series / Events

Layer 8
Analytics / PHM / AI / RAG

Layer 9
Digital Twin / Simulation

Layer 10
Operations / KPI / Governance
```

---

# 5. Capability Classification

Each source component should be classified as:

```text
CORE
Required architectural capability

CORE CANDIDATE
Recommended default implementation

OPTIONAL ADAPTER
Replaceable implementation

DOMAIN SPECIALIST
Strong for a particular industry/protocol

RESEARCH / REFERENCE
Useful reference or experimental component

EXTERNAL / COMMERCIAL
Non-core external system
```

---

# 6. Source Component Classification

| Component | Capability | Proposed Classification |
|---|---|---|
| FUXA | Web SCADA / HMI | Core Candidate |
| AVEVA PI System | Industrial historian ecosystem | External / Commercial Adapter |
| OpenRVDAS | Real-time acquisition framework | Optional / Domain Specialist |
| TcOpen | IEC 61131-3 object-oriented automation | Core Candidate / PLC Integration |
| Apache RocketMQ | Messaging / event streaming | Core Candidate |
| Quasar | Async programming | Optional |
| Matter | Device connectivity | Domain Specialist |
| OpenMUC | Industrial protocol/data framework | Core Candidate |
| jSML | Smart Message Language | Protocol Adapter |
| j62056 | IEC 62056-21 | Protocol Adapter |
| jMBus | M-Bus / Wireless M-Bus | Protocol Adapter |
| jEEBus SHIP/SPINE | EEBUS connectivity | Protocol Adapter |
| libiec61850 | IEC 61850 MMS/GOOSE/SV | Protocol Adapter |
| Java IEC 61850 stack | IEC 61850 | Alternative Adapter |
| j60870 | IEC 60870-5-104 | Protocol Adapter |
| jSCADA | SCADA reference | Research / Reference |
| OpenSCADA | SCADA/HMI | Alternative HMI |
| Archimedes | Control-system deployment | Optional |
| VOLTTRON | Distributed controls / energy | Domain Specialist |
| OpenEMS | Energy management | Domain Specialist |
| Rapid SCADA | SCADA/HMI | Alternative HMI |
| TANGO | Scientific control system | Domain Specialist |
| PyScada | Django-based SCADA | Alternative HMI |
| SQLAlchemy | Data access | Core Candidate for Python services |
| Scada-LTS | SCADA/HMI | Alternative HMI |
| SCADA preprocessing / PHM library | Fault detection / predictive maintenance | AI/PHM Candidate |

---

# 7. Open-Source-First Backbone

Recommended baseline:

```text
FUXA
+
OpenMUC
+
Apache RocketMQ
+
PostgreSQL
+
Apache IoTDB or equivalent open historian
+
FastAPI
+
Python AI/PHM services
+
Qdrant or equivalent open vector store
+
Grafana-compatible observability layer
+
Keycloak-compatible OIDC identity
+
Docker / Kubernetes where appropriate
```

The exact database/historian implementation remains replaceable.

---

# 8. HMI / SCADA Strategy

Recommended:

```text
Primary HMI:
FUXA

Alternatives:
Rapid SCADA
Scada-LTS
PyScada
OpenSCADA

Specialized:
TANGO
```

JFXSCADA should define a common HMI API so dashboards are not the source of truth.

---

# 9. HMI Boundary

The HMI should consume:

```text
Current Tag State
Alarm State
Historian Queries
Asset Metadata
Command Capabilities
User Permissions
```

It should not own:

```text
Protocol Drivers
Authoritative Historian
AI Models
PLC Logic
Safety Interlocks
```

---

# 10. Canonical Asset Model

```yaml
asset:
  id: pump_P101
  type: centrifugal_pump
  site: plant_A
  area: process_1
  parent: skid_01
  attributes:
    manufacturer: "..."
    model: "..."
  capabilities:
    - start_stop
    - speed_control
  criticality: high
```

---

# 11. Canonical Point / Tag Model

```yaml
point:
  id: plantA.process1.P101.discharge_pressure
  asset: pump_P101
  type: analog
  engineering_unit: bar
  source:
    protocol: opcua_or_adapter
    endpoint: edge_gateway_01
  limits:
    high: configured
    high_high: configured
  quality: good
```

---

# 12. Canonical Sample

```yaml
sample:
  point_id: plantA.process1.P101.discharge_pressure
  timestamp: "..."
  value: 6.4
  quality: good
  source_timestamp: "..."
  gateway_timestamp: "..."
```

---

# 13. Canonical Alarm

```yaml
alarm:
  id: alarm_0001
  point_id: plantA.process1.P101.discharge_pressure
  type: high_high
  state: active
  severity: critical
  started_at: "..."
  acknowledged: false
```

---

# 14. Canonical Command

```yaml
command:
  id: cmd_001
  target: pump_P101
  action: start
  requested_by: operator_12
  requested_at: "..."
  approval:
    required: true
  safety_profile: process_start
```

---

# 15. Command State Machine

```text
REQUESTED
   ↓
AUTHORIZED
   ↓
VALIDATED
   ↓
SENT
   ↓
ACKNOWLEDGED
   ↓
VERIFIED
   ↓
COMPLETED
```

Failure states:

```text
REJECTED
TIMEOUT
FAILED
CANCELLED
```

---

# 16. Protocol Gateway Architecture

```text
Field Protocol
     ↓
Protocol Driver
     ↓
OpenMUC / Edge Adapter
     ↓
Canonical Point
     ↓
Event / Historian
```

---

# 17. IEC 61850 Integration

Use `libiec61850` and/or a Java IEC 61850 stack behind:

```text
IEC61850Provider
```

Capabilities:

```text
MMS
GOOSE
Sampled Values
Data Models
Reports
Controls
```

For protection use cases, safety and timing requirements remain outside general AI control.

---

# 18. IEC 60870-5-104 Integration

`j60870` can expose:

```text
IEC104Provider
├── connect()
├── read_points()
├── receive_events()
├── send_command()
└── disconnect()
```

Map telecontrol information objects into the canonical point/event model.

---

# 19. IEC 62056 Integration

`j62056` can support meter/utility-device communication behind:

```text
MeterProvider
```

Use cases:

- smart meters;
- utility telemetry;
- billing/consumption data;
- energy analytics.

---

# 20. M-Bus Integration

`jMBus` provides the M-Bus / Wireless M-Bus integration path.

```text
Meter / Sensor
    ↓
jMBus
    ↓
Canonical Point
    ↓
Historian
```

---

# 21. SML Integration

`jSML` maps Smart Message Language into:

```text
SML Message
    ↓
Parser
    ↓
Meter Reading
    ↓
Canonical Sample
```

---

# 22. EEBUS Integration

SHIP/SPINE implementations can be placed behind:

```text
EEBUSProvider
```

Potential use:

- energy devices;
- smart premises;
- demand-response contexts;
- home/building energy interoperability.

---

# 23. Matter Integration

Matter should remain a specialized building/device connectivity path.

```text
Matter Device
    ↓
Matter Gateway
    ↓
Asset / Point Mapping
    ↓
JFXSCADA
```

It should not replace industrial protocols such as IEC 61850 or IEC 60870.

---

# 24. PLC / IEC 61131-3 Integration

TcOpen can provide an object-oriented automation bridge.

Recommended split:

```text
PLC
IEC 61131-3 deterministic logic
       ↓
TcOpen / PLC Adapter
       ↓
JFXSCADA supervisory layer
```

JFXSCADA should not duplicate PLC control logic.

---

# 25. Supervisory vs Control Boundary

```text
SCADA
Monitoring
Supervision
Setpoints
High-Level Commands
      ↓
PLC / Controller
Deterministic Control
Interlocks
Sequencing
      ↓
Actuator
```

---

# 26. SIS / Protection Boundary

Never model:

```text
AI Agent
   ↓
Direct SIS override
```

Prefer:

```text
AI Recommendation
      ↓
Operator / Engineering Review
      ↓
Approved Supervisory Action
```

Safety instrumented and protection functions remain independently engineered.

---

# 27. Event Streaming

Apache RocketMQ can serve as the source-compendium default for:

```text
Telemetry Events
Alarm Events
Command Events
Asset Events
Maintenance Events
AI Findings
Digital Twin Events
```

---

# 28. Event Envelope

```yaml
event:
  id: evt_0001
  type: alarm.activated
  source: edge_gateway_01
  asset: pump_P101
  timestamp: "..."
  correlation_id: process_123
  payload:
    alarm_id: alarm_0001
```

---

# 29. Topic Strategy

```text
telemetry.{site}.{area}
alarm.{site}.{area}
command.{site}.{area}
maintenance.{site}
ai.finding.{site}
twin.event.{site}
```

Keep sensitive commands on stricter authorization paths than general telemetry topics.

---

# 30. Historian Architecture

The source README references AVEVA PI as an integrated industrial data portfolio.

In the open-source-first architecture, treat it as:

```text
Optional External Historian Adapter
```

and provide an open historian interface.

---

# 31. Open Historian Interface

```text
HistorianProvider
├── write_samples()
├── query_range()
├── query_aggregate()
├── query_events()
├── get_latest()
└── retention_policy()
```

---

# 32. Open Historian Candidates

Recommended categories:

```text
Apache IoTDB
→ industrial/IoT time-series candidate

PostgreSQL
→ metadata, alarms, events, configuration

Object Storage
→ files, reports, large raw data

Optional:
other open time-series engines
```

Avoid binding analytics to proprietary historian query languages.

---

# 33. Historian Data Lifecycle

```text
Raw High-Frequency Data
       ↓
Hot Storage
       ↓
Aggregation
       ↓
Warm Storage
       ↓
Long-Term Archive
```

Retention should be configurable per point class.

---

# 34. Data Quality Model

```text
GOOD
BAD
UNCERTAIN
STALE
SUBSTITUTED
MANUAL
```

AI training should preserve quality flags rather than silently treating every sample as valid.

---

# 35. Time Synchronization

Industrial analytics should track:

```text
Device Timestamp
Gateway Timestamp
Server Timestamp
Clock Quality
Time Zone
```

For critical systems, time synchronization design must be explicit.

---

# 36. Alarm Management

```text
Measurement
   ↓
Alarm Rule
   ↓
Alarm State
   ↓
Priority
   ↓
Notification
   ↓
Acknowledgement
   ↓
Response
   ↓
Clear / Shelve / Investigate
```

---

# 37. Alarm Flood Analytics

AI may assist with:

- alarm grouping;
- flood detection;
- sequence mining;
- nuisance-alarm discovery;
- probable root-cause ranking.

It should not suppress critical alarms autonomously without policy.

---

# 38. Alarm Correlation

```text
Alarm Stream
     ↓
Temporal Correlation
     ↓
Asset Topology
     ↓
Process Context
     ↓
Candidate Root Cause
```

---

# 39. AI / PHM Plane

Recommended capabilities:

```text
Anomaly Detection
Fault Classification
Remaining Useful Life
Predictive Maintenance
Forecasting
Root-Cause Assistance
Alarm Correlation
Setpoint Advisory
Energy Optimization
Operator Copilot
```

---

# 40. AI Architecture

```text
Historian / Event Stream
       ↓
Feature Pipeline
       ↓
Model Inference
       ↓
Finding
       ↓
Evidence / Confidence
       ↓
Operator / Workflow
```

---

# 41. AI Finding Contract

```yaml
ai_finding:
  id: finding_001
  asset: wind_turbine_07
  type: anomaly
  model: vibration_autoencoder_v3
  score: 0.91
  confidence: high
  evidence:
    - bearing_temperature
    - vibration_rms
  recommended_action:
    type: inspect
```

---

# 42. FACT vs INFERENCE

Maintain explicit separation:

```text
SENSOR FACT
value measured by device

SCADA STATE
processed supervisory state

MODEL INFERENCE
AI interpretation

RECOMMENDATION
proposed response

COMMAND
authorized supervisory action

CONTROL ACTION
PLC/controller execution
```

---

# 43. Anomaly Detection

Potential open-source model stack:

```text
scikit-learn
PyTorch
River
statsmodels
custom scientific Python
```

Models should be packaged behind a common inference API.

---

# 44. Streaming Anomaly Detection

```text
Telemetry
   ↓
Window / Feature Builder
   ↓
Online Model
   ↓
Anomaly Score
   ↓
Threshold / Policy
   ↓
Finding Event
```

---

# 45. Predictive Maintenance

```text
Condition Data
    ↓
Feature Engineering
    ↓
Health Indicator
    ↓
Degradation Model
    ↓
RUL / Failure Risk
    ↓
Maintenance Recommendation
```

---

# 46. PHM Source Alignment

The current source compendium already references SCADA-data preprocessing for:

```text
Prognostics
Health Management
Wind-Turbine Fault Detection
```

JFXSCADA should generalize this into a reusable PHM pipeline.

---

# 47. PHM Interface

```text
PHMProvider
├── preprocess()
├── detect_fault()
├── estimate_health()
├── predict_rul()
├── explain()
└── get_model_metadata()
```

---

# 48. Root Cause Analysis

```text
Abnormal Condition
      ↓
Alarm Sequence
      +
Asset Topology
      +
Historian Context
      +
Maintenance History
      ↓
RCA Engine
      ↓
Ranked Hypotheses
```

AI RCA remains advisory.

---

# 49. Forecasting

Applicable to:

```text
Energy Demand
Production Demand
Temperatures
Pressures
Flow
Equipment Load
Renewable Generation
Inventory-Linked Utilities
```

---

# 50. Energy Optimization

OpenEMS and VOLTTRON can be integrated as domain-specific energy-management engines.

```text
SCADA Telemetry
      ↓
Energy State
      ↓
OpenEMS / VOLTTRON
      ↓
Optimization / Scheduling
      ↓
Approved Setpoints
```

---

# 51. OpenEMS Role

Recommended:

```text
Energy Management Domain Adapter
```

Use for:

- distributed energy;
- storage;
- PV;
- charging;
- power optimization;
- energy scheduling.

---

# 52. VOLTTRON Role

Recommended:

```text
Distributed Building / Energy Control Platform Adapter
```

Use for:

- distributed agents;
- building energy;
- transactive-energy research;
- supervisory optimization.

---

# 53. TANGO Role

TANGO can remain a specialized control-system profile for:

- laboratories;
- scientific facilities;
- accelerator-like environments;
- instrumentation-heavy research systems.

---

# 54. OpenRVDAS Role

OpenRVDAS can remain a domain-specialized acquisition option.

Recommended use:

```text
Research / vessel / instrumentation acquisition
       ↓
OpenRVDAS Adapter
       ↓
Canonical Point/Event Model
```

---

# 55. Digital Twin Architecture

```text
Physical Asset
     ↓ telemetry
Asset Twin
     ├── current state
     ├── configuration
     ├── health
     ├── operating mode
     ├── maintenance
     └── simulation parameters
     ↓
Simulation / Prediction
```

---

# 56. Twin Registry

```yaml
digital_twin:
  id: twin_pump_P101
  asset: pump_P101
  model_type: hydraulic
  model_version: 4
  calibration_version: 12
  simulation_endpoint: "..."
  telemetry_mapping:
    pressure: plantA.process1.P101.discharge_pressure
```

---

# 57. Twin Update Loop

```text
SCADA Data
   ↓
Calibration
   ↓
Twin State
   ↓
Simulation
   ↓
Expected Behavior
   ↓
Compare to Actual
   ↓
Residual / Anomaly
```

---

# 58. CAS Integration

The existing repository lifecycle uses CAS for end-to-end simulation.

Recommended:

```text
SCADA Scenario
      ↓
CAS Simulation
      ↓
Synthetic Telemetry
      ↓
JFXSCADA
      ↓
Alarm / AI / Operator Logic Test
```

---

# 59. Hardware-in-the-Loop

```text
Simulated Plant
      ↓
Real PLC / Controller
      ↓
JFXSCADA
      ↓
HMI / Alarm / Historian
```

Useful for validating supervisory integration before live deployment.

---

# 60. AI Operator Copilot

```text
Operator Question
       ↓
Local LLM / VLM
       ↓
RAG
       ↓
SCADA Context Tools
       ↓
Evidence-Based Answer
```

Potential tasks:

- explain alarms;
- summarize events;
- retrieve procedures;
- compare historical incidents;
- draft shift reports;
- suggest diagnostic checks.

---

# 61. RAG Knowledge Sources

```text
Operating Procedures
P&IDs / engineering documents
Equipment Manuals
Alarm Philosophy
Maintenance Manuals
Historical Incidents
Work Orders
Asset Metadata
Control Narratives
SCADA Configuration
```

---

# 62. RAG Data Boundary

Do not allow RAG to silently transform:

```text
Manual text
```

into:

```text
automatic control authority
```

RAG informs operators and planners only.

---

# 63. Vector Store

An open vector store such as Qdrant can support:

- semantic alarm search;
- incident similarity;
- procedure retrieval;
- maintenance knowledge;
- engineering-document RAG.

The vector database is not the authoritative historian.

---

# 64. AI Tool Plane

MCP can expose supervised SCADA tools.

Examples:

```text
get_asset
get_point
get_current_value
query_history
list_active_alarms
get_alarm_context
run_twin_simulation
get_maintenance_history
create_shift_summary
```

---

# 65. Restricted MCP Tools

Do not expose general-purpose agents directly to:

```text
raw_write_register
disable_interlock
override_protection
trip_breaker_without_policy
modify_PLC_program
disable_alarm
```

---

# 66. Supervisory Command Tool

If command tools are needed:

```text
request_command
```

not:

```text
direct_actuator_write
```

Flow:

```text
AI / Operator
    ↓
Command Request
    ↓
RBAC / Policy
    ↓
Interlock Validation
    ↓
Human Approval when required
    ↓
SCADA Command Gateway
    ↓
PLC / RTU
```

---

# 67. Identity & Access Management

Roles may include:

```text
Viewer
Operator
Senior Operator
Maintenance
Engineer
Control Engineer
Administrator
Auditor
AI Service Account
```

Use least privilege.

---

# 68. Site / Area Segmentation

```text
Enterprise
   ↓
Site
   ↓
Area
   ↓
Cell / Unit
   ↓
Asset
```

Permissions should respect this hierarchy.

---

# 69. OT / IT Segmentation

Recommended conceptual zones:

```text
L0/L1
Sensors / Actuators / Control

L2
PLC / HMI / Local SCADA

L3
Site Operations / Historian

DMZ
Controlled Integration

L4/L5
Enterprise / Analytics / Cloud
```

Exact segmentation must follow the site's cybersecurity architecture.

---

# 70. Edge Architecture

```text
Field Protocols
      ↓
Edge Gateway
      ├── protocol drivers
      ├── buffering
      ├── normalization
      ├── quality
      ├── local rules
      └── store-and-forward
      ↓
Central JFXSCADA
```

---

# 71. Store-and-Forward

When connectivity fails:

```text
Read Field Data
      ↓
Local Buffer
      ↓
Connection Restored
      ↓
Ordered Replay
      ↓
Historian
```

Maintain original timestamps.

---

# 72. Edge AI

Suitable:

```text
Fast Anomaly Detection
Local Vision
Equipment Health
Data Compression
Signal Quality
```

Not every AI model needs to run centrally.

---

# 73. Edge AI Deployment Contract

```yaml
edge_model:
  id: motor_vibration_v4
  target: gateway_07
  runtime: onnx
  input_points:
    - vibration_x
    - vibration_y
  output:
    - anomaly_score
```

---

# 74. Model Registry

```yaml
model:
  id: pump_failure_v5
  task: fault_classification
  version: 5
  training_dataset: pump_history_v12
  metrics:
    f1: 0.91
  deployment:
    approved_sites:
      - plant_A
```

---

# 75. Model Governance

Track:

```text
Dataset Version
Feature Version
Model Version
Approval
Deployment
Drift
Retirement
```

---

# 76. Model Drift

```text
Production Data
     ↓
Distribution Monitor
     ↓
Drift?
  ├── No → continue
  └── Yes
       ↓
   Review / Retrain
```

Never auto-retrain and deploy consequential models without governance.

---

# 77. Data Provenance

Every derived value should preserve:

```text
Source
Timestamp
Transformation
Model
Version
Operator Override
Quality
```

---

# 78. SQLAlchemy Role

For Python microservices:

```text
FastAPI
   ↓
SQLAlchemy
   ↓
PostgreSQL
```

Use for configuration, metadata, alarms, audit and application state.

Do not force time-series telemetry through ORM row-by-row ingestion.

---

# 79. Configuration Database

Suggested entities:

```text
Site
Area
Asset
Point
AlarmRule
CommandCapability
UserRole
Dashboard
Model
Twin
ProtocolEndpoint
RetentionPolicy
```

---

# 80. Observability

Monitor JFXSCADA itself:

```text
gateway_connected
protocol_error_rate
point_update_latency
historian_write_latency
alarm_delivery_latency
command_roundtrip
event_bus_lag
model_inference_latency
AI_finding_rate
```

---

# 81. SCADA Health Dashboard

```text
Protocol Health
Gateway Health
Historian Health
Event Bus Health
HMI Health
AI Service Health
Command Gateway Health
Storage Health
```

---

# 82. Audit Trail

```yaml
audit_event:
  actor: operator_12
  action: command_request
  asset: pump_P101
  requested_value: start
  decision: approved
  approver: supervisor_02
  timestamp: "..."
```

---

# 83. Immutable Evidence

For critical operations preserve:

```text
Alarm Events
Acknowledgements
Command Requests
Approvals
Configuration Changes
Model Deployments
User/Role Changes
```

---

# 84. Configuration Versioning

```text
SCADA Configuration
      ↓
Git / Version Registry
      ↓
Review
      ↓
Validated Release
      ↓
Deployment
```

---

# 85. Infrastructure as Code

Recommended for:

```text
Containers
Deployment
Network Policy
Secrets References
Dashboards
AI Services
Historian Services
```

OT device configuration may require a separate controlled process.

---

# 86. GitHub Integration

```text
SCADA Config
AI Models Metadata
Dashboards
Documentation
Integration Tests
      ↓
GitHub
      ↓
Pull Request
      ↓
CI
      ↓
Review
      ↓
Release Artifact
```

---

# 87. GitHub MCP Role

The official GitHub MCP architecture already used elsewhere in the portfolio can support:

- repository inspection;
- issue workflows;
- PR review;
- CI diagnostics;
- security findings;
- documentation.

It should not become the OT real-time command path.

---

# 88. DevSecOps Pipeline

```text
Change
  ↓
Static Validation
  ↓
Unit Tests
  ↓
Protocol Contract Tests
  ↓
Simulation Tests
  ↓
Security Checks
  ↓
Approval
  ↓
Staged Deployment
```

---

# 89. Protocol Simulation Tests

Create synthetic endpoints for:

```text
IEC 61850
IEC 60870-5-104
M-Bus
SML
IEC 62056
EEBUS
Matter
Modbus / OPC profile where integrated
```

---

# 90. Alarm Regression Tests

```text
Synthetic Telemetry
       ↓
Alarm Rules
       ↓
Expected Alarm Sequence
       ↓
Compare
```

---

# 91. AI Regression Tests

```text
Known Historical Cases
      ↓
Model
      ↓
Expected Finding Range
      ↓
Compare
```

---

# 92. Digital Twin Regression Tests

```text
Scenario
  ↓
Twin
  ↓
Expected Dynamic Response
  ↓
Tolerance Check
```

---

# 93. MBSE Mapping

```text
Stakeholder Need
     ↓
Operational Scenario
     ↓
SCADA Function
     ↓
Asset / Point / Alarm
     ↓
Protocol
     ↓
Implementation
     ↓
Verification
```

---

# 94. MBSE → CAD → CAM → CAS

```text
MBSE
Control / monitoring architecture
      ↓
CAD
Plant / electrical / instrumentation design
      ↓
CAM
Equipment / panel / installation / assembly
      ↓
CAS
Simulation / commissioning / performance analysis
      ↓
SCADA Operations
```

---

# 95. P&ID / Engineering Integration

Potential canonical relationships:

```text
P&ID Equipment
   ↓
Asset ID
   ↓
PLC Tag
   ↓
SCADA Point
   ↓
Historian Series
   ↓
AI Feature
   ↓
Maintenance Record
```

---

# 96. Digital Thread

```text
Requirement
  ↓
Asset
  ↓
Signal
  ↓
Control Logic
  ↓
SCADA
  ↓
Historian
  ↓
AI Model
  ↓
Maintenance / KPI
```

---

# 97. JFXOSMS Integration

Microfactory integration:

```text
JFXOSMS
Microfactory Digital Twin
      ↓
Machine / Cell Telemetry
      ↓
JFXSCADA
      ↓
OEE / Alarm / PHM
      ↓
JFXAI4BPM
Business Process
```

---

# 98. Humanoid Assembly Integration

With JFXAI4MHERS:

```text
Humanoid Robot
     ↓
ROS 2 / Robot Adapter
     ↓
Cell Gateway
     ↓
JFXSCADA
     ↓
Health / Alarm / Production State
```

JFXSCADA supervises robot-cell state but does not replace the robot's local safety controller.

---

# 99. JFXFMIS Integration

Farm / agriculture:

```text
Sensors / Pumps / Irrigation / Energy
       ↓
JFXSCADA
       ↓
JFXFMIS
Farm Operations / Digital Twin
       ↓
Optimization / Scheduling
```

---

# 100. JFXAI4MESS Integration

Multi-energy system:

```text
Meters / IED / DER / Storage
       ↓
JFXSCADA
       ↓
OpenEMS / VOLTTRON
       ↓
JFXAI4MESS
Energy-System Simulation
```

---

# 101. JFXAI4BPM Integration

```text
Operational Event
      ↓
JFXSCADA
      ↓
Business Event
      ↓
JFXAI4BPM
      ↓
Workflow / Approval / Maintenance Process
```

---

# 102. Maintenance Workflow

```text
AI Finding
    ↓
SCADA Health Event
    ↓
JFXAI4BPM
    ↓
Maintenance Work Request
    ↓
Human Review
    ↓
Work Order
    ↓
Completion
    ↓
SCADA / Twin Update
```

---

# 103. JFXBSC Integration

```text
SCADA / Production Metrics
       ↓
Semantic KPI Layer
       ↓
JFXBSC
       ↓
Objectives / KPIs / OKRs
```

---

# 104. KPI Examples

```text
Availability
OEE
Energy Intensity
Alarm Rate
Critical Alarm Response Time
Maintenance Backlog
Unplanned Downtime
Mean Time Between Failures
Mean Time To Repair
Forecast Error
AI Detection Lead Time
```

---

# 105. Operator Shift Summary

```text
Shift Events
  +
Alarms
  +
Commands
  +
Production
  +
Maintenance
  ↓
AI Summarizer
  ↓
Draft Shift Report
  ↓
Operator Review
```

---

# 106. Natural-Language Query

Example:

```text
"Show all high-severity alarms on Line 2
during the 30 minutes before the compressor trip."
```

Flow:

```text
LLM
 ↓
Structured Query Plan
 ↓
SCADA/Historian Tools
 ↓
Results
 ↓
Evidence-Based Summary
```

---

# 107. No Hallucinated Plant State

The AI assistant must never invent:

```text
Current Tag Values
Alarm States
Command Results
Asset State
Historian Evidence
```

If a tool or data source fails, report the failure.

---

# 108. Command Verification

After a command:

```text
Command Sent
    ↓
Protocol Acknowledgement
    ↓
Observed State Change
    ↓
Verification
```

Do not equate transport acknowledgement with successful process action.

---

# 109. Failure Handling

```text
Protocol Failure
    ↓
Retry Policy
    ↓
Connection Recovery
    ↓
Store-and-Forward
    ↓
Alert
```

Commands require stricter retry semantics than telemetry.

---

# 110. Idempotency

Where supported:

```text
Command
  ↓
Command ID
  ↓
Duplicate Detection
  ↓
Execute Once / Verify
```

---

# 111. High Availability

Critical services:

```text
SCADA Core
Historian
Message Bus
Identity
Command Gateway
Alarm Service
```

should define:

- redundancy;
- failover;
- backup;
- restore;
- degraded modes.

---

# 112. Offline Operation

A site should remain able to perform essential supervisory functions when enterprise/cloud connectivity is unavailable.

```text
Local HMI
Local PLC
Local Alarm
Local Historian Buffer
       ↓
Later Sync
```

---

# 113. Deployment Profiles

## Profile A — Small Open SCADA

```text
FUXA
OpenMUC
PostgreSQL
FastAPI
```

## Profile B — Industrial Data Platform

```text
FUXA
OpenMUC
RocketMQ
Apache IoTDB
PostgreSQL
AI Services
```

## Profile C — Energy

```text
FUXA
OpenMUC
OpenEMS
VOLTTRON
Historian
AI Forecasting
```

## Profile D — Manufacturing

```text
FUXA
TcOpen
RocketMQ
Historian
PHM
JFXOSMS
JFXAI4BPM
```

---

# 114. External PI Integration Profile

For organizations already using AVEVA PI:

```text
JFXSCADA
  ↓
HistorianProvider
  ↓
PI Adapter
  ↓
Existing PI System
```

The same application code should also work with the open historian provider.

---

# 115. Open Historian Migration

```text
Legacy / External Historian
        ↓
Read Adapter
        ↓
Canonical Sample/Event
        ↓
Open Historian
```

Validate semantics such as:

- timestamp;
- quality;
- compression;
- interpolation;
- event frames;
- asset metadata.

---

# 116. Recommended Repository Structure

```text
jfxscada/
├── README.md
│
├── docs/
│   ├── architecture/
│   ├── protocols/
│   ├── historian/
│   ├── hmi/
│   ├── alarms/
│   ├── ai/
│   ├── digital-twin/
│   ├── security/
│   └── operations/
│
├── core/
│   ├── assets/
│   ├── points/
│   ├── alarms/
│   ├── commands/
│   └── events/
│
├── gateways/
│   ├── openmuc/
│   ├── iec61850/
│   ├── iec60870/
│   ├── iec62056/
│   ├── mbus/
│   ├── sml/
│   ├── eebus/
│   └── matter/
│
├── hmi/
│   ├── fuxa/
│   ├── rapid-scada/
│   ├── pyscada/
│   └── adapters/
│
├── historian/
│   ├── api/
│   ├── iotdb/
│   ├── postgres/
│   └── external-pi/
│
├── streaming/
│   └── rocketmq/
│
├── ai/
│   ├── anomaly/
│   ├── phm/
│   ├── forecasting/
│   ├── rca/
│   ├── rag/
│   └── model-registry/
│
├── twins/
│   ├── registry/
│   ├── simulation/
│   └── calibration/
│
├── integrations/
│   ├── mcp/
│   ├── jfxosms/
│   ├── jfxai4mhers/
│   ├── jfxfmis/
│   ├── jfxai4mess/
│   ├── jfxai4bpm/
│   └── jfxbsc/
│
└── tests/
    ├── protocols/
    ├── alarms/
    ├── historian/
    ├── ai/
    ├── twin/
    └── security/
```

---

# 117. MVP Phase 1 — SCADA Core

Implement:

```text
Asset Registry
Point Registry
FUXA
OpenMUC
PostgreSQL
Basic Alarm Service
```

Initial protocol profile:

```text
one utility protocol
+
one industrial protocol
```

---

# 118. MVP Phase 2 — Historian & Events

Add:

```text
Apache RocketMQ
Open Historian Provider
Time-Series Storage
Event Store
Trend API
```

---

# 119. MVP Phase 3 — Protocol Expansion

Add:

```text
IEC 61850
IEC 60870-5-104
M-Bus
SML
IEC 62056
EEBUS
Matter where useful
```

---

# 120. MVP Phase 4 — PHM / AI

Add:

```text
SCADA preprocessing
Anomaly Detection
Fault Classification
Predictive Maintenance
Model Registry
```

Start in advisory mode.

---

# 121. MVP Phase 5 — Digital Twin

Add:

```text
Twin Registry
Simulation Adapter
Calibration
Expected-vs-Actual
Synthetic Telemetry
```

---

# 122. MVP Phase 6 — AI Operator Copilot

Add:

```text
RAG
Vector Store
Local LLM
MCP Read Tools
Shift Summaries
Alarm Explanation
```

Read-only first.

---

# 123. MVP Phase 7 — Controlled Command Workflow

Add:

```text
Command Requests
RBAC
Approvals
Policy Gateway
Audit
State Verification
```

AI remains advisory unless a narrowly defined command workflow is explicitly approved.

---

# 124. MVP Phase 8 — Portfolio Integration

Add:

```text
JFXOSMS
JFXAI4MHERS
JFXFMIS
JFXAI4MESS
JFXAI4BPM
JFXBSC
```

---

# 125. Initial Production Profile

Recommended first production-oriented stack:

```text
FUXA
+
OpenMUC
+
libiec61850 / j60870 / meter adapters as required
+
Apache RocketMQ
+
Apache IoTDB
+
PostgreSQL
+
FastAPI
+
Python PHM
+
Qdrant
+
local LLM runtime
+
OIDC / RBAC
+
Docker
```

---

# 126. Alternative HMI Profiles

```text
FUXA
→ default web HMI

Rapid SCADA
→ alternative full SCADA profile

PyScada
→ Python/Django profile

Scada-LTS
→ Java/web profile

OpenSCADA
→ alternative/reference
```

---

# 127. Alternative Historian Profiles

```text
Open profile:
Apache IoTDB

Metadata:
PostgreSQL

External enterprise:
AVEVA PI adapter
```

The core must depend on the `HistorianProvider` contract, not a specific vendor.

---

# 128. Alternative Messaging Profiles

Source default:

```text
Apache RocketMQ
```

Possible future adapter categories:

```text
NATS
Kafka-compatible systems
MQTT brokers
```

Only add alternatives when a concrete deployment requirement exists.

---

# 129. Open AI Stack Profile

```text
Python
scikit-learn / PyTorch / River
MLflow-style experiment tracking
ONNX for edge deployment where useful
Qdrant for RAG
local LLM runtime
FastAPI
MCP Gateway
```

---

# 130. Decision Matrix for New Dependencies

Evaluate:

```text
Open License
Active Maintenance
Protocol Coverage
Industrial Adoption
Offline Capability
Security
HA Support
Performance
Observability
API Stability
Data Portability
Hardware Independence
```

---

# 131. Architecture Anti-Pattern

Avoid:

```text
HMI
  tightly coupled to
One Historian
  tightly coupled to
One Protocol Driver
  tightly coupled to
One AI Vendor
```

Prefer:

```text
HMI Adapter
+
Historian Provider
+
Protocol Provider
+
AI Provider
+
Canonical Data Model
```

---

# 132. Final Reference Architecture

```text
                           JFXSCADA
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
             HMI            OPERATIONS        REPORTS
            FUXA             Alarms            KPIs
              │                │                │
              └────────────────┼────────────────┘
                               ▼
                     CANONICAL DATA MODEL
                               │
           ┌───────────────────┼───────────────────┐
           ▼                   ▼                   ▼
        HISTORIAN          EVENT STREAM          AI / PHM
      Apache IoTDB          RocketMQ      Anomaly / RUL / RCA
           │                   │                   │
           └───────────────────┼───────────────────┘
                               ▼
                         DIGITAL TWIN
                               │
                               ▼
                      COMMAND / POLICY GATE
                               │
     ┌─────────────┬───────────┼────────────┬─────────────┐
     ▼             ▼           ▼            ▼             ▼
 IEC 61850     IEC 60870    Metering      EEBUS        Matter
libiec61850     j60870    jSML/j62056    SHIP/SPINE
     │             │           │            │             │
     └─────────────┴───────────┼────────────┴─────────────┘
                               ▼
                       OpenMUC / EDGE
                               │
                               ▼
                  PLC / RTU / IED / METER
                               │
                               ▼
                      PHYSICAL PROCESS
```

---

# 133. Cross-Portfolio Architecture

```text
                     JFXBSC
                  KPI / Strategy
                       ▲
                       │
JFXAI4BPM ◄────── JFXSCADA ──────► JFXOSMS
Processes            │             Microfactory
                     │
          ┌──────────┼───────────┐
          ▼          ▼           ▼
      JFXFMIS   JFXAI4MESS   JFXAI4MHERS
        Farm      Energy       Humanoids
```

---

# 134. Key Design Principle

> **Use open industrial protocol adapters and OpenMUC at the edge; a canonical asset/tag/event model as the integration contract; FUXA as a replaceable HMI; RocketMQ as the source-aligned event backbone; an open historian abstraction rather than a proprietary dependency; and an AI/PHM plane that remains advisory and evidence-driven, with every command passing through deterministic OT authorization and safety controls.**

---

# 135. Source vs Recommendation Boundary

This document deliberately distinguishes:

```text
SOURCE PROJECT FACT
→ components explicitly listed in the current JFXSCADA README

ARCHITECTURE RECOMMENDATION
→ proposed interfaces, providers, AI stack and integration design

OPTIONAL ADDITION
→ components introduced to fill an architectural capability
```

The current source README does not state that all named projects are installed together or form one executable distribution.

---

# 136. Open-Source / Licensing Boundary

The preferred architecture is:

> **open, modular and standards-oriented, designed to minimize lock-in and allow independent implementations.**

However, individual components may use different open-source licenses and the source compendium also includes external/commercial systems such as AVEVA PI System.

Before distribution, verify:

- project license;
- protocol-stack license;
- redistribution requirements;
- commercial-use restrictions;
- embedded third-party libraries;
- trademark usage;
- device/vendor SDK terms.

---

# 137. Safety & Operations Disclaimer

SCADA systems can influence physical equipment, electrical systems and industrial processes.

Production deployments require qualified engineering for:

- control;
- protection;
- functional safety;
- electrical safety;
- cybersecurity;
- process safety;
- alarm management;
- commissioning;
- change management;
- regulatory compliance.

AI and language-model components should remain outside hard real-time and safety-critical control loops unless independently engineered, verified and approved for that purpose.

