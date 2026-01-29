# Architecture

## Components

### Microsoft Sentinel
- Detects RBAC role assignments via AzureActivity logs
- Creates incidents using a scheduled analytic rule
- Hosts automation rules and playbook integration

### Azure Logic App (Stateful)
- Triggered by Sentinel incident updates
- Uses System Assigned Managed Identity
- Executes remediation via ARM REST API

### Azure Resource Manager
- Authoritative control plane for RBAC
- Deletes role assignments via `Microsoft.Authorization/roleAssignments/delete`

---

## Architecture Diagram (Conceptual)

┌────────────────────────────┐
│ Azure Subscription │
│ RBAC Assignment Created │
└─────────────┬──────────────┘
│
▼
┌────────────────────────────┐
│ AzureActivity Logs │
└─────────────┬──────────────┘
│
▼
┌────────────────────────────┐
│ Sentinel Analytic Rule │
│ RBAC Privilege Escalation │
└─────────────┬──────────────┘
│
▼
┌────────────────────────────┐
│ Sentinel Incident │
│ Tag: RBAC-AutoRemove │
└─────────────┬──────────────┘
│
▼
┌────────────────────────────┐
│ Logic App Playbook │
│ Managed Identity │
└─────────────┬──────────────┘
│
▼
┌────────────────────────────┐
│ ARM DELETE Role Assignment │
└────────────────────────────┘

---

## Trust Boundaries

| Boundary | Purpose |
|--------|--------|
| Sentinel Workspace | Detection & orchestration |
| Subscription RBAC | Authorization enforcement |
| Managed Identity | Secure automation identity |

