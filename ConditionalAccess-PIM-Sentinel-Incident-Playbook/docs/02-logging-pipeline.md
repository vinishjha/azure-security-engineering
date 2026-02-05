## Logging pipeline

Entra ID logs are sent to Log Analytics using Diagnostic Settings.

Diagnostic setting name:
- `entra-id-audit-signin-sentinel`

Data sources:
- AuditLogs
- SigninLogs

Sentinel queries the Log Analytics workspace; the Content Hub installs content but does not by itself guarantee data ingestion.
