# SPL Workspace — Scenario 01 DNS Reconnaissance & Enumeration

**Status:** Planned — no detection logic is considered final before baseline and controlled testing.

When real searches exist, preserve them as:

```text
spl/
├── baseline.spl
├── hunting.spl
├── detection.spl
└── validation.spl
```

## Purpose

- **baseline.spl** — measure ordinary activity before the simulation;
- **hunting.spl** — analyst pivots that explain the raw behavior;
- **detection.spl** — final tuned scenario detection;
- **validation.spl** — normal-vs-scenario tests and final acceptance checks.

## Rules

- Use the real telemetry fields observed in this lab.
- Keep thresholds evidence-based and record tuning reasons.
- Test false positives deliberately.
- Keep detection logic readable enough for another team member to reproduce.
- Do not copy arbitrary thresholds from a public example or generate a rule around what the AI model prefers.
