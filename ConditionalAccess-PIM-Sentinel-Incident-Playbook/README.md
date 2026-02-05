# Conditional Access + PIM + Sentinel: Identity Control Integrity (Project 4)

This project demonstrates identity security engineering in Microsoft Entra ID using:

- **Conditional Access** (Tier-0 protection)
- **Privileged Identity Management (PIM)** (just-in-time privilege)
- **RBAC governance** (detecting standing privilege / permanent privileged assignments)
- **Microsoft Sentinel** (custom detections + incidents)
- **SOAR (Logic Apps)** for safe incident enrichment (non-destructive automation)

This repository is **NDA-safe** and focuses on control intent, operational workflow, and evidence.

---

## What this project proves

1. **Tier-0 Conditional Access guardrails** can be monitored for drift (create/modify/disable/delete).
2. **PIM activation** events are detectable and operationalized via Sentinel incidents.
3. **Standing privilege** (permanent privileged role assignment) is detectable as an identity control violation.
4. Automation is applied **safely** (enrichment + tagging) with **least privilege**, avoiding risky auto-remediation.

---

## Architecture (high level)

**Entra ID (AuditLogs + SigninLogs)**  
→ Diagnostic Settings (**entra-id-audit-signin-sentinel**)  
→ Log Analytics Workspace  
→ Microsoft Sentinel Analytics Rules  
→ Sentinel Incidents  
→ Automation Rule → Logic App Playbook (enrichment + tags)

---

## Contents

- `docs/` — control intent and implementation notes
- `kql/` — final KQL detections used in Sentinel analytics rules
- `playbook/` — playbook expressions and notes
- `screenshots/` — evidence of controls, detections, incidents, and playbook runs

---

## Key detections

- **CA Tier-0 drift**: alert when Tier-0 Conditional Access is changed
- **PIM privileged role activation**: alert when privileged access becomes active
- **Permanent privileged role assignment**: alert on standing privilege (“permanent”)

See `kql/` for exact queries and `screenshots/04-sentinel-analytics-rules/` for rule configuration proof.

---

## Automation (SOAR)

The playbook intentionally performs **non-destructive automation**:
- Adds a structured incident comment
- Applies consistent tags: `Identity-Control`, `P04`, `Automated-Enrichment`

See `docs/04-soar-playbook.md` and `screenshots/06-playbook-runs/`.

---

## Evidence

Start here: `docs/05-validation-evidence.md`
