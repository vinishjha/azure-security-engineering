
---

# 📄 detection.md

```md
# Detection Logic — Key Vault Suspicious Secret Access

## Data Source

- AzureDiagnostics
- Resource Provider: MICROSOFT.KEYVAULT
- Category: AuditEvent

---

## KQL Logic

```kql
AzureDiagnostics
| where Category == "AuditEvent"
| where ResourceProvider == "MICROSOFT.KEYVAULT"
| where OperationName in ("SecretGet","KeyGet","CertificateGet")
| where ResultType == "Success"
| extend PrincipalOID = tostring(identity_claim_oid_g)
| extend VaultResourceId = tostring(ResourceId)
| project TimeGenerated, PrincipalOID, VaultResourceId
| order by TimeGenerated desc
| take 1
