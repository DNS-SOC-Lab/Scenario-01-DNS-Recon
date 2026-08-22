# AI Integration — Scenario 01 DNS Reconnaissance & Enumeration

**Status:** **✅ Scenario-specific engineering integration validated**  
**Shared AI foundation:** Reused from `DNS-Lab-Infrastructure`  
**Scenario profile:** `dns_recon_v1`

This folder contains only the Scenario 01 mapping required after the human-facing detection fields stabilized. The shared Flask/OpenAI/HEC platform is not duplicated here.

## Flow

```text
Scenario 01 detection v1.0
      ↓
Scheduled Splunk alert
      ↓
Native webhook result
      ↓
Scenario 01 evidence normalization
      ↓
Shared AI bridge
      ↓
Structured OpenAI response
      ↓
Internal HTTPS HEC
      ↓
index=dns_soc_ai
      ↓
Human analyst validation
```

## Principle

AI is **advisory**.

The detection is created and validated from Route 53 evidence before the LLM is involved. The analyst keeps direct access to the raw Splunk events, and the indexed AI result retains `human_validation_required=true`.

## Scenario-specific artifact

[`scenario-01-ai-mapping.md`](scenario-01-ai-mapping.md) documents:

- Scenario identity;
- the final alert contract;
- `evidence_json` content;
- bridge normalization;
- AI result extraction in Splunk;
- the end-to-end validation result.
