# AI Profile — Scenario 01 DNS Reconnaissance & Enumeration

**Status:** Planned — the shared AI bridge is common infrastructure and is built outside this repository.

This folder will contain only the scenario-specific payload/profile mapping once the final detection has stable fields.

## Expected workflow

```text
Stable Splunk detection
      ↓
Structured scenario payload
      ↓
Shared Flask / LLM bridge
      ↓
Structured AI summary
      ↓
Splunk index=dns_soc_ai
      ↓
Human SOC validation against raw evidence
```

## Rules

- AI assistance never becomes the source of truth.
- Do not let the LLM decide whether containment happens.
- Preserve the input payload and returned summary as evidence.
- Record where the AI was correct, incomplete or wrong.
- Add a real scenario profile here only after the detection fields are finalized.
