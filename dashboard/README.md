# Dashboard Plan — Scenario 01 DNS Reconnaissance & Enumeration

**Status:** Planned. Build from real telemetry after the scenario baseline fields are confirmed.

## Design goal

The dashboard is an investigation surface, not decoration. Every panel must answer a SOC question and use real project fields.

## Shared controls

Start with one **Time Range** input shared by every panel. Add only useful scenario filters such as client/source, query type, response code, domain or VPC/instance identity.

## Planned layout

- Shared time range plus source/query-type/response filters where useful;
- KPIs: total queries, unique queried names, distinct query types, observed sources, NXDOMAIN/other result count;
- DNS queries over time and query-type distribution;
- Top queried names/subdomains and response-code/result distribution;
- Nginx follow-up requests from the same investigation window;
- VPC Flow follow-up context and an analyst-ready raw-event table;

A useful common shape is:

```text
Input bar: time + scenario filters
Row 1: 4–6 SOC summary KPIs
Row 2: behavior over time
Row 3: DNS pattern/distribution views
Row 4: network/Web/endpoint correlation
Row 5: investigation table
Row 6: response verification when applicable
```

## Quality rules

- Use actual source/sourcetype/field names from the lab.
- Do not depend on a pre-labelled `classification="Suspicious"` training-data field.
- Baseline comes before detection panels are finalized.
- Prefer drilldowns or linked searches that take the analyst to raw evidence.
- Keep the visual language consistent and readable across all four scenarios.
- Save the final dashboard XML/export here only after it is tested.

## Final artifacts later

Expected after implementation:

```text
dashboard/
├── README.md
└── scenario-01-dashboard.xml   # or the actual exported dashboard format used
```
