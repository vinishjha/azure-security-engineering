# Sentinel Detections  
## Control-Plane Drift & Privilege Escalation

This folder contains **Microsoft Sentinel analytics rule designs** that monitor for **high-impact security events** in a regulated Azure landing zone.

Each detection is:
- intentionally scoped
- built on authoritative telemetry
- mapped to a real security control objective
- backed by evidence

---

## Detection Philosophy

Rather than detecting “attacks,” these rules detect **unsafe changes to the security posture itself**.

In regulated environments, the most damaging incidents often begin with:
- a misconfigured network rule
- a policy exemption
- a privileged role assignment

These detections focus on exactly those events.

---

## Included Detections

### 1. NSG Internet Exposure — Inbound Allow

**Objective:**  
Detect when a Network Security Group rule introduces **direct internet exposure** that violates a private-by-default posture.

**Why this matters:**  
In regulated environments, internet exposure is often allowed *only when explicit and reviewed*. Silent exposure is a critical failure mode.

**Signal source:**  
- AzureActivity  
- `Microsoft.Network/networkSecurityGroups/securityRules/write`

**Detection logic highlights:**  
- Inbound rules
- `Allow` access
- Risky ports (e.g., 22, 3389, 445, database ports)
- Broad source prefixes (`*`, `Internet`, `0.0.0.0/0`)

---

### 2. Policy Guardrail Bypass — Policy Exemption Change

**Objective:**  
Detect creation or modification of **Azure Policy exemptions**, which can bypass enforced guardrails.

**Why this matters:**  
Policy exemptions are a legitimate feature — and a common abuse path.  
In regulated environments, exemptions must be **visible, reviewable, and auditable**.

**Signal source:**  
- AzureActivity  
- `Microsoft.Authorization/policyExemptions/write`
- `Microsoft.Authorization/policyExemptions/delete`

---

### 3. RBAC Privilege Escalation — Subscription Role Assignment

**Objective:**  
Detect **privileged RBAC role assignments** at subscription scope.

**Why this matters:**  
Subscription-level role assignments (Owner, Contributor, User Access Administrator) are **Tier-0 events** that can enable:
- policy bypass
- logging suppression
- persistence
- full environment takeover

**Signal source:**  
- AzureActivity  
- `Microsoft.Authorization/roleAssignments/write`

---

## Why AzureActivity Was Chosen

All detections rely on **AzureActivity** because it is:
- authoritative
- control-plane native
- centrally ingestible
- auditable

Sentinel incident payloads alone are insufficient for this class of detection.

---

## Evidence-Driven Design

Every detection in this folder has corresponding proof in: detections/security-evidence/


This includes:
- analytics rule screenshots
- query results
- generated incidents
- platform context

These detections are **not theoretical** — they were executed, triggered, and validated.


