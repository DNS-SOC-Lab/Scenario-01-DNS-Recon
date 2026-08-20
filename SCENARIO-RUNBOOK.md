# Scenario 01 Runbook — DNS Reconnaissance & Enumeration

**Status:** Planned — begins after the shared AI foundation is complete  
**Primary MITRE ATT&CK:** T1590.002 — Gather Victim Network Information: DNS

This is the working checklist for the scenario. Sections remain marked **Planned** until real implementation or evidence exists. Do not fill gaps with invented values.

## 1. Objective

Generate authorized DNS reconnaissance against the lab namespace and determine whether the SOC can distinguish normal DNS activity from concentrated enumeration across record types/names, including any follow-up interaction with the discovered Web target.

**Status:** Planned.

## 2. Architecture

Describe only the systems and paths actually used when the scenario is prepared.

```text
Shared infrastructure
      ↓
Scenario-specific infrastructure (if any)
      ↓
Controlled simulation
      ↓
Telemetry
      ↓
Splunk / AI / Human SOC / IR
```

**Scenario infrastructure dependency:** No additional scenario-specific AWS infrastructure is expected. Reuse `dns-attack01`, Route 53 public authority/query logs, `dns-soc-web01`, Splunk, VPC Flow Logs, CloudTrail/Resolver context where useful, and the shared AI bridge.

**Status:** Planned.

## 3. Prerequisites

Before execution, confirm:

- required shared infrastructure is healthy;
- scenario-specific infrastructure, if any, has been built and validated;
- telemetry is arriving with usable timestamps/fields;
- the shared AI bridge is available;
- the Project Lead has the approved simulation plan and ground-truth clock;
- Detection Engineer has captured the normal baseline before attack activity;
- SOC Analyst and IR/Defender know the expected exercise window but do not receive the final detection answer in advance.

**Status:** Planned.

## 4. Attack / Simulation

Record the final controlled commands/tools, safety limits and start/end timestamps only when the exercise is executed.

The simulation must stay inside owned/authorized lab infrastructure and match the scenario objective.

**Status:** Planned.

## 5. Telemetry

List the real sources that capture this scenario. Candidate shared sources include:

- Route 53 public authoritative query logs;
- AWS VPC Resolver Query Logs;
- team-controlled resolver logs from Scenario 02 onward;
- VPC Flow Logs;
- Nginx access telemetry when Web follow-up is relevant;
- CloudTrail when control-plane changes are part of the evidence;
- endpoint/client telemetry only where it is actually collected.

Do not force unused telemetry into the scenario.

**Status:** Planned.

## 6. Detection

Detection focus:

- record-type diversity across A/AAAA/MX/NS/TXT and other observed types;
- query rate and unique-name count by observed source;
- authoritative result/response context;
- DNS-to-Nginx follow-up correlation;
- VPC Flow context for follow-up network activity;

Write the final behavioral hypothesis after observing baseline data.

**Status:** Planned.

## 7. SPL / Detection Logic

Use files under [`spl/`](spl/) after real searches exist:

```text
baseline.spl
hunting.spl
detection.spl
validation.spl
```

Thresholds must be tuned from real baseline and controlled testing rather than copied from an example.

**Status:** Planned.

## 8. Alert

The final alert should contain enough evidence for a human analyst to start investigation without guessing field meanings.

Recommended common fields:

- detection name/version;
- first/last event time;
- observed client/source identity;
- query count / unique-name count / scenario-specific metric;
- relevant query names/types/results;
- supporting Web/network context;
- severity and rationale;
- raw-event or drilldown search.

**Status:** Planned.

## 9. AI Triage

The shared AI bridge is reused. This repository adds only the scenario profile/payload mapping after the detection fields are stable.

Record:

- payload sent;
- AI summary returned;
- useful observations;
- missing/incorrect claims;
- what the SOC Analyst verified independently.

**Status:** Planned.

## 10. SOC Analysis

Build the human investigation timeline from raw evidence. Document pivots, competing explanations, disposition and confidence.

The AI output is supporting context only.

**Status:** Planned.

## 11. Incident Response

Containment is evidence-driven and should be proportionate to the controlled reconnaissance. The IR/Defender documents the human decision, any approved restriction, and the verification result rather than applying a control automatically.

Record the approved decision and who performed it.

**Status:** Planned.

## 12. Evidence

Store structured evidence notes under [`evidence/`](evidence/) and screenshots under [`screenshots/`](screenshots/).

Evidence should cover:

- pre-scenario health/baseline;
- ground-truth timing;
- telemetry;
- detection/alert;
- AI output;
- SOC analysis;
- containment;
- verification.

**Status:** Planned.

## 13. Containment

Containment is performed only after the human investigation reaches the scenario's approved response condition.

Do not treat detection or AI output as automatic authorization.

**Status:** Planned.

## 14. Verification

Prove what changed after response. Use before/after DNS, network, Web or endpoint evidence appropriate to the scenario.

**Status:** Planned.

## 15. Results

Summarize the final outcome after the exercise:

- detection result;
- SOC disposition;
- response result;
- verification result;
- whether the scenario completion condition passed.

**Status:** Planned.

## 16. MITRE ATT&CK Mapping

Primary mapping: **T1590.002 — Gather Victim Network Information: DNS**.

Review the mapping against the behavior that was actually generated and detected. Add no extra techniques unless evidence supports them.

If a Cyber Kill Chain view is included, record only the phase that the implemented behavior genuinely demonstrates and cite the scenario evidence for that choice.

**Status:** Planned.

## 17. False Positives

Test plausible benign activity that resembles part of the scenario. Record each threshold or logic change and why it improved separation.

**Status:** Planned.

## 18. Lessons Learned

Capture useful technical, detection, analyst and IR lessons. Write them as reusable engineering knowledge rather than a chat/debugging transcript.

**Status:** Planned.

## 19. Reproduction Instructions

At completion, provide a clean ordered path:

```text
Prerequisites
→ scenario infrastructure
→ baseline
→ controlled simulation
→ validate telemetry
→ run detection
→ investigate
→ AI comparison
→ contain
→ verify
→ cleanup/reset
```

**Status:** Planned.

## 20. Screenshots

Use descriptive filenames and show screenshots next to the sections they prove. Keep a compact screenshot index in [`screenshots/README.md`](screenshots/README.md).

**Status:** Planned.

## Network & protocol view

- Layer 7 DNS: names, record types, authoritative responses/results;
- Layer 4: DNS plus HTTPS follow-up where generated;
- Layer 3: public source/destination and VPC Flow context;
- Application: Nginx access after DNS discovery;

This section should be updated with the actual fields/ports seen during execution.

## Completion gate

Final detection separates baseline from controlled reconnaissance, the alert contains analyst-ready evidence, the AI profile is validated against raw events, and the team completes investigation → response → verification with screenshots and lessons learned.
