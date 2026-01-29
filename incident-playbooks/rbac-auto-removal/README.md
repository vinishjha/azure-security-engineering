# RBAC Privilege Escalation – Automated Remediation Playbook

## Overview

This project demonstrates a **production-grade Sentinel SOAR playbook** that automatically removes **high-risk Azure RBAC role assignments** (Owner, Contributor, User Access Administrator) **after explicit human approval**, using Microsoft Sentinel, Azure Logic Apps, and Managed Identity.

The playbook closes a critical SOC gap:
> Sentinel can detect RBAC privilege escalation — but does not provide a native remediation mechanism.

This solution bridges that gap safely and audibly.

---

## Threat Scenario

1. A privileged RBAC role is assigned at the subscription scope
2. Azure Activity logs record the change
3. Sentinel detects the event and creates an incident
4. A SOC analyst reviews the incident
5. Analyst adds the tag `RBAC-AutoRemove`
6. The playbook removes the role assignment automatically
7. Sentinel incident is updated with audit comments

---

## Why This Matters

RBAC privilege escalation is one of the **highest-impact control-plane threats** in Azure.
Manual remediation does not scale and often lags detection.

This playbook demonstrates:
- Control-plane security automation
- Human-in-the-loop approval
- Managed Identity–based remediation
- Clean auditability

---

## High-Level Flow

Azure RBAC Change
↓
AzureActivity Logs
↓
Sentinel Analytic Rule
↓
Sentinel Incident
↓
Human Approval (Tag)
↓
Logic App Playbook
↓
ARM DELETE roleAssignment

---

## Key Features

- No secrets (Managed Identity only)
- No blind automation (tag-gated)
- No brittle alert parsing
- Full audit trail in Sentinel
- Safe against infinite loops

---

## Related Documentation

- [Privileged Access Escalation Detection](../../privileged-access-escalation.md)

---

## Evidence & Screenshots

| Screenshot | Description |
|----------|-------------|
| 01 | Sentinel incident with RBAC escalation alert |
| 02 | Automation rule triggering on tag |
| 03 | Logic App playbook design and successful run |
| 04 | Managed Identity RBAC at subscription scope |
| 05 | Managed Identity Sentinel permissions |
| 06 | Activity log showing role created → deleted |

All remediation actions were executed by the Logic App managed identity.


