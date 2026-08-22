# Scenario 01 Runbook — DNS Reconnaissance & Enumeration

**Overall scenario status:** Official SOC/IR exercise pending  
**Detection Engineering status:** **✅ Complete / Detection Engineering Ready**  
**Primary MITRE ATT&CK:** `T1590.002 — Gather Victim Network Information: DNS`  
**Detection Engineer:** [Sonia](https://github.com/sonia11mansha415)

This runbook follows the shared 20-area scenario documentation standard while separating the work that has already been implemented from the official synchronized exercise that is still pending.

> [!IMPORTANT]
> Detection Engineering validation traffic is not the official Scenario 01 exercise. The completed engineering work proved the telemetry, dashboard, detection, alert, drilldown and AI evidence path. SOC final disposition, Incident Response and containment/verification remain later team phases.

## Status summary

| # | Area | Current status |
|---:|---|---|
| 1 | Objective | ✅ Defined |
| 2 | Architecture | ✅ Ready — shared lab reused |
| 3 | Prerequisites | ✅ Detection Engineering prerequisites passed; exercise-day health reconfirmation pending |
| 4 | Attack / Simulation | ⏳ Official exercise pending; engineering validation traffic complete |
| 5 | Telemetry | ✅ Validated |
| 6 | Detection | ✅ Complete |
| 7 | SPL / Detection Logic | ✅ Complete |
| 8 | Alert | ✅ Complete and validated |
| 9 | AI Triage | ✅ Engineering integration validated; official analyst comparison pending |
| 10 | SOC Analysis | ⏳ Pending official exercise |
| 11 | Incident Response | ⏳ Pending official exercise |
| 12 | Evidence | ✅ Detection Engineering evidence complete; official exercise evidence pending |
| 13 | Containment | ⏳ Pending human decision in official exercise |
| 14 | Verification | ⏳ Pending any approved response |
| 15 | Results | 🟡 Detection Engineering result complete; full scenario result pending |
| 16 | MITRE ATT&CK Mapping | ✅ `T1590.002` confirmed for implemented behavior |
| 17 | False Positives | ✅ Engineering validation complete |
| 18 | Lessons Learned | ✅ Detection Engineering lessons documented |
| 19 | Reproduction Instructions | ✅ Detection Engineering path documented; official exercise path to be completed later |
| 20 | Screenshots | ✅ Curated Detection Engineering set complete |

---

## 1. Objective

Detect and investigate concentrated DNS enumeration against the controlled `soclab.abdul4rehman215.tech` namespace while separating it from ordinary public DNS activity.

**Engineering learning goal:** build the rule from real Route 53 behavior rather than from arbitrary example thresholds.

**Status:** ✅ Defined and implemented for Detection Engineering.

## 2. Architecture

Scenario 01 uses the shared platform; no additional scenario-specific AWS infrastructure was required for Detection Engineering.

```text
dns-attack01 / public resolvers
          ↓
Route 53 authoritative DNS
          ↓
Route 53 public query logs
          ↓
Kinesis
          ↓
Splunk Enterprise
     ↙           ↘
Dashboard       Detection / Alert
                     ↓
                 AI bridge
                     ↓
                 dns_soc_ai
```

Shared Nginx, VPC Flow, CloudTrail and Resolver telemetry remain available as supporting context, but are not forced into the detection when they do not improve the evidence.

**Status:** ✅ Ready / validated.

## 3. Prerequisites

Detection Engineering prerequisites already proven:

- `index=dns_soc_aws` receiving Route 53 authoritative events;
- `sourcetype="aws:kinesis"` stable and searchable;
- required raw DNS structure mapped;
- current ingestion timing measured;
- baseline captured before threshold selection;
- Splunk Dashboard Studio working;
- scheduled alert capability validated;
- shared AI bridge available for final mapping.

Before the **official exercise**, reconfirm current health and synchronize the exercise clock/team roles.

**Status:** ✅ Engineering prerequisite gate passed / official exercise-day check pending.

## 4. Attack / Simulation

### Detection Engineering traffic already completed

Small controlled queries were used only to validate the rule:

```text
20 queries
4 controlled names
5 record types: A, AAAA, MX, NS, TXT
```

This was enough to prove the detection path without running a large simulation.

### Official exercise

The official synchronized Scenario 01 simulation, ground-truth start/end timestamps and Project Lead execution record are still pending.

**Status:** ⏳ Official exercise pending.

## 5. Telemetry

### Primary source used by the detection

```text
index=dns_soc_aws
sourcetype=aws:kinesis
```

Validated Route 53 fields:

```text
query_name
query_type
response_code
protocol
edge_location
observed_dns_source
edns_client_subnet
```

`observed_dns_source` remains neutral because authoritative DNS evidence does not automatically prove original endpoint attribution.

### Supporting sources available

- Nginx Web telemetry;
- VPC Flow Logs;
- CloudTrail;
- Resolver Query Logs where useful.

**Status:** ✅ Validated for the Detection Engineering use case.

## 6. Detection

Final behavioral hypothesis:

```text
same observed DNS source
+ controlled lab namespace
+ concentrated short-window activity
+ query-name breadth
+ meaningful record-type diversity
→ possible DNS reconnaissance
```

Final threshold:

```text
query_count >= 16
unique_names >= 4
distinct_query_types >= 3
```

NXDOMAIN is context, not a mandatory condition.

**Status:** ✅ Detection v1.0 complete.

## 7. SPL / Detection Logic

Implemented artifacts:

- [`spl/baseline.spl`](spl/baseline.spl)
- [`spl/hunting.spl`](spl/hunting.spl)
- [`spl/detection.spl`](spl/detection.spl)
- [`spl/validation.spl`](spl/validation.spl)

Threshold selection came from the observed baseline and controlled validation rather than an external fixed value.

**Status:** ✅ Complete.

## 8. Alert

Final scheduled alert:

```text
Scenario 01 - Possible DNS Reconnaissance
cron:            * * * * *
lookback:        Last 3 minutes
trigger:         Number of Results > 0
trigger mode:    Once
throttle:        60 seconds
severity:        Medium
actions:         Triggered Alerts + Webhook
```

The alert produces an analyst-ready evidence row and was validated through automatic trigger history.

See [`spl/scheduled-alert.md`](spl/scheduled-alert.md).

**Status:** ✅ Complete and validated.

## 9. AI Triage

Scenario-specific fields were mapped into the shared bridge only after detection v1.0 stabilized.

```text
alert_id
alert_name
scenario
severity
event_time
source
evidence_json
```

The end-to-end engineering validation succeeded:

```text
scheduled alert
→ webhook
→ AI bridge
→ OpenAI HTTP 200
→ internal HEC
→ index=dns_soc_ai
```

The result retains `human_validation_required=true`.

The official SOC Analyst comparison of AI output against official exercise evidence is still pending.

**Status:** ✅ Engineering integration validated / ⏳ official exercise comparison pending.

## 10. SOC Analysis

The dashboard, alert evidence and raw-event pivots are ready for the SOC Analyst.

The official investigation timeline, competing hypotheses, confidence and final disposition will be recorded during the synchronized exercise.

**Status:** ⏳ Pending.

## 11. Incident Response

The IR/Defender will respond only after human investigation reaches the scenario's approved response condition.

No Detection Engineering test or AI output automatically authorizes containment.

**Status:** ⏳ Pending.

## 12. Evidence

Detection Engineering evidence is documented in:

- [`DETECTION-ENGINEERING.md`](DETECTION-ENGINEERING.md)
- [`evidence/detection-engineering-validation.md`](evidence/detection-engineering-validation.md)
- [`screenshots/`](screenshots/)

Official exercise ground truth, SOC findings, IR actions and verification evidence will be added later.

**Status:** ✅ Detection Engineering evidence complete / ⏳ official exercise evidence pending.

## 13. Containment

No official containment action has been performed as part of the completed Detection Engineering phase.

Containment remains evidence-driven and human-approved.

**Status:** ⏳ Pending.

## 14. Verification

Detection Engineering verification already proved telemetry → detection → alert → AI flow.

Response verification, if containment is approved in the official exercise, must compare the pre-response and post-response technical state.

**Status:** ⏳ Official response verification pending.

## 15. Results

### Detection Engineering result

**PASS.** Scenario 01 is Detection Engineering Ready.

The implementation has a validated dashboard, hunting searches, final detection, scheduled alert, raw evidence pivot and AI integration path.

### Full scenario result

Not yet assigned. It depends on the official SOC/IR exercise.

**Status:** 🟡 Engineering result complete / full scenario pending.

## 16. MITRE ATT&CK Mapping

Primary mapping:

**`T1590.002 — Gather Victim Network Information: DNS`**

The controlled engineering behavior enumerated the project DNS namespace using multiple DNS record types and names. No additional ATT&CK technique is added without evidence.

The best-fit Cyber Kill Chain stage for this behavior is **Reconnaissance**.

**Status:** ✅ Confirmed for implemented Detection Engineering behavior.

## 17. False Positives

The baseline showed that record-type diversity can be high in ordinary/background public DNS activity, so diversity alone was rejected as a sufficient signal.

Final validation proved:

- controlled multi-name/multi-record-type behavior crosses the rule;
- basic benign A/AAAA activity remains below it.

See [`spl/validation.spl`](spl/validation.spl).

**Status:** ✅ Engineering validation complete.

## 18. Lessons Learned

Key lessons are preserved in [`DETECTION-ENGINEERING.md`](DETECTION-ENGINEERING.md), including:

- field semantics before attribution;
- ingestion health before detection changes;
- baseline before thresholds;
- positive and benign testing with the same rule;
- reversible troubleshooting before destructive recovery;
- schema validation as a separate integration boundary;
- human validation of AI output.

**Status:** ✅ Detection Engineering lessons complete.

## 19. Reproduction Instructions

### Detection Engineering reproduction path

```text
1. Confirm Route 53 events in dns_soc_aws / aws:kinesis
2. Map the authoritative DNS fields
3. Measure _time vs _indextime
4. Run baseline.spl on a clean normal period
5. Import / review the Dashboard Studio JSON
6. Run the two hunts in hunting.spl
7. Run detection.spl against baseline traffic
8. Generate one small authorized recon-like test burst
9. Confirm the detection row
10. Generate the minimal benign test
11. Confirm no detection row
12. Run validation.spl
13. Configure / validate the scheduled alert
14. Confirm the triggered evidence row and raw-event pivot
15. Validate the Scenario 01 AI contract and dns_soc_ai result
```

Do not substitute the engineering test for the official synchronized exercise.

**Status:** ✅ Detection Engineering path documented.

## 20. Screenshots

Curated evidence is indexed in [`screenshots/README.md`](screenshots/README.md).

The public repository intentionally excludes repetitive progress screenshots, covered result panes, minor syntax mistakes and trial-and-error that does not add reusable engineering knowledge.

**Status:** ✅ Detection Engineering set curated.

---

## Official exercise handoff

Detection Engineering hands the later SOC/IR exercise these ready components:

```text
trusted DNS telemetry
final dashboard
detection v1.0
scheduled alert
analyst evidence fields
raw-event pivot
AI assistance path
```

The next phase should add only the evidence that genuinely comes from the official synchronized exercise.
