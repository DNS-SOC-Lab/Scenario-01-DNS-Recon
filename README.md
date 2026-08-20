# Scenario 01 — DNS Reconnaissance & Enumeration

**Status:** Planned — begins after the shared AI foundation is complete  
**Primary MITRE ATT&CK:** T1590.002 — Gather Victim Network Information: DNS

## Objective

Generate authorized DNS reconnaissance against the lab namespace and determine whether the SOC can distinguish normal DNS activity from concentrated enumeration across record types/names, including any follow-up interaction with the discovered Web target.

## Infrastructure dependency

No additional scenario-specific AWS infrastructure is expected. Reuse `dns-attack01`, Route 53 public authority/query logs, `dns-soc-web01`, Splunk, VPC Flow Logs, CloudTrail/Resolver context where useful, and the shared AI bridge.

The shared AWS/Splunk platform is not rebuilt inside this repository. Any new AWS resource is designed in the infrastructure project and documented there after it exists.

## Detection focus

- record-type diversity across A/AAAA/MX/NS/TXT and other observed types;
- query rate and unique-name count by observed source;
- authoritative result/response context;
- DNS-to-Nginx follow-up correlation;
- VPC Flow context for follow-up network activity;

## Network & protocol view

- Layer 7 DNS: names, record types, authoritative responses/results;
- Layer 4: DNS plus HTTPS follow-up where generated;
- Layer 3: public source/destination and VPC Flow context;
- Application: Nginx access after DNS discovery;

DNS is Layer 7 evidence, but the scenario should correlate it with the Layer 3/4, endpoint, cloud or application evidence that actually helps prove the behavior.

## Planned dashboard

The dashboard should follow one shared time range and lead the analyst from summary → behavior → correlation → raw evidence.

- Shared time range plus source/query-type/response filters where useful;
- KPIs: total queries, unique queried names, distinct query types, observed sources, NXDOMAIN/other result count;
- DNS queries over time and query-type distribution;
- Top queried names/subdomains and response-code/result distribution;
- Nginx follow-up requests from the same investigation window;
- VPC Flow follow-up context and an analyst-ready raw-event table;

See [`dashboard/README.md`](dashboard/README.md) for the planned layout.

## Team

| Role | Member |
|---|---|
| Project Lead / Attack Simulation | Abdul-Rehman |
| SOC Analyst | Musfira |
| Detection Engineer | Sonia |
| IR / Defender | Lubaba |

## Scenario workflow

This repository follows the common 20-part standard:

**Objective → Architecture → Prerequisites → Simulation → Telemetry → Detection → SPL → Alert → AI Triage → SOC Analysis → IR → Evidence → Containment → Verification → Results → MITRE → False Positives → Lessons → Reproduction → Screenshots.**

The working checklist is [`SCENARIO-RUNBOOK.md`](SCENARIO-RUNBOOK.md).

## Repository map

```text
.
├── README.md                 # scenario overview and locked design
├── SCENARIO-RUNBOOK.md       # 20-part execution/documentation checklist
├── dashboard/                # dashboard plan, later final XML/export
├── spl/                      # baseline, hunting, detection and validation SPL
├── ai/                       # scenario profile/payload mapping for shared AI bridge
├── ir/                       # response/containment/verification record
├── evidence/                 # structured ground truth and evidence notes
└── screenshots/              # curated visual evidence
```

The folders are prepared now, but fake implementation artifacts are not. Real `.spl`, dashboard XML, AI profiles and evidence are added only when they have been built and tested.

## Shared project references

- [DNS Lab Infrastructure](https://github.com/DNSentinel-Lab/DNS-Lab-Infrastructure) — shared AWS, DNS, Splunk and AI foundation
- [Scenario infrastructure roadmap](https://github.com/DNSentinel-Lab/DNS-Lab-Infrastructure/blob/main/00-project-design/scenario-infrastructure-roadmap.md) — future EC2/DNS/network changes owned by the infrastructure repository
- [Scenario documentation standard](https://github.com/DNSentinel-Lab/DNS-Lab-Infrastructure/blob/main/00-project-design/scenario-documentation-standard.md) — common 20-part SOC workflow, dashboard and evidence rules

## Completion condition

Final detection separates baseline from controlled reconnaissance, the alert contains analyst-ready evidence, the AI profile is validated against raw events, and the team completes investigation → response → verification with screenshots and lessons learned.
