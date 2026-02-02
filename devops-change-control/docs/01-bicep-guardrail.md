# Policy-as-code Guardrail: Deny Public Storage Exposure

## Why this exists
In regulated environments, we assume deployment automation can be misused or misconfigured.
Guardrails must exist outside CI/CD so that unsafe configs are denied at the platform layer.

## Implementation
- Bicep template attempts to set:
  - publicNetworkAccess = Enabled
  - allowBlobPublicAccess = true
- Azure Policy assignment `Deny-Storage-Public-Exposure` is scoped to the production RG.

## Evidence
See `devops-change-control/evidence/01-policy-deny/`:
- policy assignment
- pipeline deploy failure with `RequestDisallowedByPolicy`
