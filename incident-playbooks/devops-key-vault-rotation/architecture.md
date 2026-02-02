# Architecture

This document explains how I designed the system and why each component exists.

---

## Components I used

### 1) Azure Key Vault (Tier-0 secret store)
I stored the CI/CD secret in Key Vault because it provides:
- strong access control (RBAC),
- full audit logging of secret access,
- versioning for safe rotation and containment.

### 2) Azure DevOps Pipeline (legitimate secret consumer)
I used an Azure DevOps pipeline to represent legitimate, expected secret access.
This is important architecturally because detection must distinguish:
- expected CI/CD reads,
- suspicious or unexpected reads.

### 3) Log Analytics Workspace (central logging layer)
I routed Key Vault diagnostics into Log Analytics so I can:
- query and detect Key Vault secret access behavior,
- run analytics rules on a schedule,
- enrich playbook context from logs.

### 4) Microsoft Sentinel Analytics Rule (detection + incident creation)
I created an analytics rule to:
- detect Key Vault `SecretGet` events for a specific Tier-0 secret,
- generate incidents that represent actionable security events,
- become the trigger point for automation.

### 5) Logic App Playbook (automated response + audit trail)
I used a Logic App playbook to perform containment actions and record evidence.
Architecturally, this playbook is the “response engine” that:
- reduces mean time to contain,
- performs safe and repeatable actions,
- documents everything directly inside the incident.

---

## Data flow I designed

1) A secret is accessed → Key Vault produces audit telemetry  
2) Logs are ingested into Log Analytics (`AzureDiagnostics`)  
3) The Sentinel analytics rule runs on a schedule → creates an incident  
4) The playbook triggers from the incident → enriches context and contains  
5) The playbook updates the incident → comment, tags, and status reflect actions taken  

---

## Why I used secret versions for containment

Key Vault secret versioning allows me to:
- keep full audit history (do not delete),
- invalidate older material quickly by disabling older versions,
- preserve the newest version while reducing reuse of older versions.

This is a practical containment pattern because it limits attacker reuse while keeping evidence intact.
