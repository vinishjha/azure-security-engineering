# Detection (Sentinel Analytics Rule + KQL)

This document explains how I built detection for suspicious Key Vault secret reads.

---

## Detection goal

My goal was to detect Key Vault `SecretGet` activity for my Tier-0 secret and turn it into a Sentinel incident that can trigger automation.

---

## Data source I used

I used Key Vault diagnostic audit logs stored in Log Analytics:
- Table: `AzureDiagnostics`
- Provider: `MICROSOFT.KEYVAULT`
- Category: `AuditEvent`
- Operation: `SecretGet`

---

## Baseline KQL (show me everything for this secret)

I use this baseline query when I want full visibility (no exclusions):

```kql
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.KEYVAULT"
| where Category == "AuditEvent"
| where OperationName == "SecretGet"
| extend requestUri = tostring(requestUri_s)
| extend afterSecrets = tostring(split(requestUri, "/secrets/")[1])
| extend afterSecrets = tostring(split(afterSecrets, "?")[0])
| extend SecretName = tostring(split(afterSecrets, "/")[0])
| where SecretName == "cicd-pipeline-secret"
| extend CallerOid   = tostring(column_ifexists("identity_claim_oid_g",""))
| extend CallerAppId = tostring(column_ifexists("identity_claim_appid_g",""))
| extend CallerUPN   = tostring(column_ifexists("identity_claim_upn_s",""))
| project TimeGenerated, SecretName, CallerOid, CallerAppId, CallerUPN, CallerIPAddress, requestUri
| order by TimeGenerated desc
