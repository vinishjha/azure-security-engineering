## Architectural Context (What This Repository Represents)

📁 **`landing-zone-architecture/`**

This repository is built on top of a **deliberate, security-owned Azure landing zone architecture** designed for **regulated environments** (aerospace, industrial, critical infrastructure).

Before any playbooks, pipelines, or detections were created, the foundation was designed so that **security is enforced by architecture**, not by manual processes or human trust.

At a high level, this architecture establishes:

- **Clear separation of duties** between platform security and application workloads  
- **Identity as the primary control plane**, not the network  
- **Preventative guardrails before detection**, not alert-only security  
- **Centralized, tamper-resistant evidence collection** owned by security  
- **Explicit blast-radius containment** for failure and compromise scenarios  

### Foundational Design Principles

- **Platform Security vs Workload Isolation**  
  Security controls (logging, policy, detection, automation) live in a dedicated platform subscription that workloads cannot disable or bypass.

- **Identity-First Trust Model**  
  Platform identities, workload identities, and pipeline identities are intentionally scoped and separated. Authorization is explicit and auditable.

- **Intentional Network Exposure**  
  Network exposure is allowed only when explicit, observable, and reviewable — not accidental or implicit.

- **Centralized Logging as Evidence**  
  Telemetry is collected centrally using enforced ingestion paths and retained as durable security evidence, not just troubleshooting logs.

- **Assume Breach, Control Blast Radius**  
  The architecture assumes compromise is possible and focuses on limiting impact through subscription boundaries, policy enforcement, and independent security ownership.

### Why This Matters

All projects in this repository — DevOps change control, Sentinel detections, SOAR playbooks, identity enforcement, and guardrails — are **downstream of this foundation**.

They are not bolt-on controls.

They work **because** the architecture enforces:
- who can act
- where they can act
- how far actions can propagate
- and how every action is recorded

This repository should be read as a **security platform story**, not a collection of scripts.




---

# Azure Security Engineering Portfolio  
## Preventative Controls, SOAR Enforcement & Regulated Cloud Architecture (NDA-Safe)

---

## Overview

This repository is a **portfolio-grade, NDA-safe collection of Azure security engineering projects** that demonstrate how **regulated enterprises (aerospace / industrial / critical infrastructure)** design, enforce, and operate cloud security controls.

The work here reflects **how I build security systems in practice** as a **high mid-level Azure Security Engineer** — not demos, not tutorials, and not vendor marketing examples.

The projects progress deliberately from:

1. **Preventative platform controls (guardrails & change control)**  
2. **Detection → response → enforcement playbooks (SOAR)**  
3. **Tier-0 identity and architecture integrity protection**

Together, they show **end-to-end ownership of cloud security posture**.

---

## NDA-Safe Disclosure Statement

- No proprietary employer systems  
- No internal tooling or naming  
- No production secrets or identifiers  
- No customer data  

All designs use **public Azure services, documented APIs, and reproducible patterns**.

This repository demonstrates **how I think and engineer**, not where I work.

---

## How to Read This Repository

This repository has **two primary pillars**, each representing a different but connected security responsibility:

1. **Preventative Security Architecture (Guardrails & Change Control)**  
2. **Reactive & Enforced Security Response (Incident Playbooks / SOAR)**  

You can review either pillar independently, but together they represent a **complete security lifecycle**.

---

## Repository Structure (High Level)

├── devops-change-control/
├── incident-playbooks/
└── README.md


---

# Pillar 1 — Preventative Security Architecture  
## DevOps Change Control & Guardrails

📁 `devops-change-control/`

### Why This Exists

In regulated environments, **security cannot rely on detection alone**.  
Unsafe infrastructure changes must be **prevented before they reach production**, even if they are approved by humans.

This pillar focuses on **preventative controls** that operate **outside and above CI/CD**.

---

### What This Pillar Demonstrates

- DevOps treated as a **privileged security boundary**
- Human approval ≠ authorization
- Azure Policy as **enforced guardrails**
- Infrastructure as Code under governance
- Centralized audit telemetry beyond Azure DevOps
- Honest documentation of real-world failures

---

### Key Capabilities Shown

- Azure DevOps pipelines with:
  - What-If (advisory)
  - Manual approval gates
  - Enforced production deployment
- Deny-based Azure Policy (not audit-only)
- Log Analytics custom tables for approval telemetry
- Modern Azure Monitor ingestion (DCE / DCR)
- Policy enforcement surfaced as security signals

This pillar answers the question:

> **“How do we make sure insecure changes never exist?”**

---

# Pillar 2 — Reactive & Enforced Security Response  
## Incident Playbooks (Sentinel SOAR)

📁 `incident-playbooks/`

### Why This Exists

Even with strong guardrails, **security incidents still happen**:
- privilege escalation
- credential misuse
- data access anomalies
- identity drift

This pillar focuses on **how regulated enterprises respond safely and decisively** using **Sentinel SOAR** — without breaking production.

---

### What This Pillar Demonstrates

- High-signal detection engineering (KQL)
- Sentinel analytics rules and automation
- Logic App playbooks with enforcement logic
- Human-in-the-loop vs automatic response decisions
- Least-privilege IAM modeling for automation
- Evidence-driven incident handling

These are **not auto-close playbooks**.  
They are **enforcement-grade controls**.

---

## Incident Playbook Projects

### Project 1 — RBAC Privilege Escalation (Subscription Scope)
Detects and contains **Tier-0 RBAC role assignments** using:
- AzureActivity telemetry
- Tag-based human approval
- ARM API enforcement

---

### Project 2 — Key Vault RBAC Containment
Responds to **secret access events** by:
- identifying the principal and vault
- removing vault-scope RBAC assignments
- documenting containment in Sentinel

---

### Project 3 — Tier-0 CI/CD Secret Auto-Rotation
Protects CI/CD supply chain credentials by:
- classifying Tier-0 secrets
- safely rotating Key Vault secret versions
- preventing automation loops
- documenting eradication actions

---

### Project 4 — Identity & Architecture Integrity Enforcement
Protects the **security architecture itself**, including:
- Conditional Access drift
- PIM misuse or permanence
- Landing zone guardrail bypass
- Tier-0 vs Tier-1 enforcement decisions

This project demonstrates **platform security ownership**.

---

## Common Engineering Principles (Across All Work)

- Preventative > detective wherever possible
- Control-plane telemetry first
- Identity treated as Tier-0
- Managed Identity over secrets
- Explicit permission modeling
- Idempotent, fail-safe automation
- Evidence before claims

---

## Why This Matters in Aerospace & Regulated Environments

Aerospace security requires:
- separation of duties
- enforced guardrails
- immutable audit trails
- predictable automation behavior
- architecture-level integrity

The combined projects in this repository show:
- how unsafe states are prevented
- how incidents are contained or eradicated
- how identity and platform drift are controlled
- how security systems withstand audits

---

## What This Repository Says About the Engineer

A reviewer should conclude:

- This engineer understands Azure deeply (control plane, identity, policy)
- This engineer designs **preventative controls**, not just alerts
- This engineer builds SOAR that is safe, reversible, and auditable
- This engineer can operate in **regulated, high-impact environments**
- This engineer documents failures honestly and learns from them

---

## Final Note

This repository is intentionally structured as a **security architecture portfolio**, not a code dump.

Every folder exists to answer one question:

> **“Can this person be trusted to protect production in a regulated cloud?”**
