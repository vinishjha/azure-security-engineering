# Defender-first Triage Story (Sentinel-backed)

## Scenario
An automated deployment attempt in prod was blocked by Azure Policy (`RequestDisallowedByPolicy`).

## Why it matters
- Could indicate a compromised pipeline, incorrect IaC change, or attempted bypass
- High-signal control-plane activity in regulated environments

## Triage steps
1. Confirm scope: subscription + resource group targeted
2. Identify actor: pipeline identity (service principal) from AzureActivity `Caller`
3. Correlate: look for nearby actions from the same `Caller` (RBAC writes, policy exemptions, diagnostic settings changes)
4. Containment: disable service connection / revoke federated credentials temporarily
5. Lessons: update IaC and enforce approvals for policy exemption changes

## Evidence
See `evidence/03-sentinel-detection/` and `evidence/02-devops-policy-enforcement/`.
