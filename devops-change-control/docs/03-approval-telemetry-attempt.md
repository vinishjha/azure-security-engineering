# Approval Telemetry (Attempted) — Cross-System Evidence Design

## Why we attempted this
A common security requirement is:
> “Alert if a production deployment occurs without an approval.”

Azure Activity Logs show “a deployment happened” but do NOT contain Azure DevOps approval state.
To correlate “approval happened” with “deployment happened,” approval signals must be exported from DevOps into the same analytics plane (Log Analytics / Sentinel).

## Architecture we designed
- Create a custom log table `DevOpsApprovals_CL` in Log Analytics
- Use Data Collection Endpoint (DCE) + Data Collection Rule (DCR) to accept a custom stream
- Emit an “approval marker” from the pipeline into that custom table

## What succeeded
- DCE created and available
- Custom table created and verified (`DevOpsApprovals_CL`)
- DCR created and updated to map `Custom-DevOpsApprovals_CL` to the LAW destination
- Pipeline step successfully authenticated but was blocked by ingestion authorization (403)

## Why it failed in this tenant
Two blocking issues were encountered:
1) The required ingestion permission role/dataAction was not available/assignable in this tenant context,
   preventing the pipeline identity from posting to the Logs Ingestion API.
2) A fallback Azure Function relay was created; ZIP deployment uploaded successfully but trigger sync failed (Bad Request),
   indicating runtime/package expectations not satisfied in the current subscription configuration.

## Enterprise note
In enterprise tenants, this pattern is typically solved via one of:
- Azure DevOps Environments + approvals (audited) combined with centralized ingestion of DevOps audit events
- A platform-owned relay (Function/App) using managed identity + supported ingestion permissions
- Standardized ingestion of Azure DevOps audit streams into SIEM (Defender/Sentinel)

## Evidence
See `devops-change-control/evidence/03-telemetry-attempt/`:
- DCE/DCR/table proof
- pipeline 403 ingestion attempt log
- function deployment/trigger sync error logs
