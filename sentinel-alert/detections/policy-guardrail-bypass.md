## Detection Example: Policy Guardrail Bypass – Policy Exemption Change

### Detection Objective

Detect creation or deletion of Azure Policy exemptions, which can bypass preventive security guardrails.

### Architectural Context

- Guardrails are enforced through Azure Policy
- Exemptions represent intentional deviations
- Deviations must be visible, reviewable, and auditable

### Signal Source

- AzureActivity
- Operations:
  - POLICYEXEMPTIONS/WRITE
  - POLICYEXEMPTIONS/DELETE

### Why This Matters

Policy exemptions are one of the most common and least monitored methods of bypassing security controls in regulated environments.

This detection ensures:
- governance transparency
- accountability for deviations
- audit-ready evidence

### Evidence

- Exemption creation: `security-evidence/screenshots/07-policy-exemption-created.png`
- Sentinel incident: `security-evidence/screenshots/08-policy-exemption-incident.png`
