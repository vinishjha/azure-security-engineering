# Azure Security Engineering Portfolio  
## Regulated Cloud Architecture, Preventative Controls & SOAR Enforcement (NDA-Safe)

---

## What This Repository Is

This repository is a **portfolio-grade, NDA-safe Azure security engineering project** that demonstrates how **regulated enterprises** (aerospace, industrial, critical infrastructure) design, enforce, and operate cloud security **as a platform**, not as ad-hoc alerts or scripts.

It represents **how I build security systems in practice** as a **high mid-level Azure Security Engineer** — not tutorials, not demos, and not vendor marketing examples.

The repository should be read **top-down**, because every layer depends on the one before it.

---

## How to Read This Repository (10-Minute Guide)

1. **Start with the architecture**  
   → `landing-zone-architecture/`

2. **See how unsafe changes are prevented**  
   → `devops-change-control/`

3. **See how security drift is detected**  
   → `sentinel-alert/`

4. **See how incidents are enforced and contained**  
   → `incident-playbooks/`

This is a **security platform story**, not a collection of scripts.

---

## NDA-Safe Disclosure Statement

- No proprietary employer systems  
- No internal tooling or naming  
- No production secrets or identifiers  
- No customer data  

All designs use **public Azure services, documented APIs, and reproducible patterns**.

This repository demonstrates **how I think and engineer**, not where I work.

---

# Architectural Context (Foundation of Everything)

📁 **`landing-zone-architecture/`**

Before any playbooks, pipelines, or detections were created, this repository was built on a **deliberate, security-owned Azure landing zone architecture** designed for **regulated environments**.

Security here is enforced by **architecture**, not by human trust or best intentions.

### What This Architecture Establishes

- **Clear separation of duties**  
  Platform security and application workloads live in separate subscriptions.

- **Identity as the primary control plane**  
  Authorization and blast radius matter more than network placement.

- **Preventative guardrails before detection**  
  Unsafe states should be blocked, not merely alerted on.

- **Centralized, tamper-resistant evidence**  
  Logging and telemetry are owned by security, not workloads.

- **Explicit blast-radius containment**  
  The architecture assumes compromise and limits impact by design.

### Foundational Design Principles

**Platform Security vs Workload Isolation**  
Security controls (policy, logging, detection, automation) live in a dedicated platform subscription that workloads cannot disable or bypass.

**Identity-First Trust Model**  
Platform identities, workload identities, and pipeline identities are intentionally separated and scoped. Authorization is explicit and auditable.

**Intentional Network Exposure**  
Public exposure is allowed only when explicit, observable, and reviewable — never accidental.

**Centralized Logging as Evidence**  
Telemetry is collected through enforced ingestion paths and retained as durable security evidence, not just troubleshooting logs.

**Assume Breach, Control Blast Radius**  
The design assumes failure is possible and focuses on limiting damage through boundaries and ownership.

### Why This Comes First

Every other project in this repository depends on this foundation.

Without it, the controls would be:
- brittle
- bypassable
- unauditable

---

# Pillar 1 — Preventative Security Architecture  
## DevOps Change Control & Guardrails

📁 **`devops-change-control/`**

### Why This Exists

In regulated environments, **human approval is not a security control**.

Unsafe infrastructure changes must be **prevented before they reach production**, even if they are reviewed and approved.

This pillar focuses on **preventative controls that operate above and outside CI/CD**.

### What This Pillar Demonstrates

- DevOps treated as a **privileged security boundary**
- Human approval ≠ authorization
- Azure Policy used as **deny-based guardrails**
- Infrastructure as Code under governance
- Centralized audit telemetry beyond Azure DevOps
- Honest documentation of real-world failures

### Key Capabilities Shown

- Azure DevOps pipelines with:
  - What-If (advisory)
  - Manual approval gates
  - Enforced production deployment
- Deny-based Azure Policy (not audit-only)
- Log Analytics custom tables for approval telemetry
- Modern Azure Monitor ingestion (DCE / DCR)
- Policy enforcement surfaced as security signals

**This pillar answers:**  
> *“How do we make sure insecure changes never exist?”*

---

# Pillar 2 — Detection of Security Drift  
## Sentinel Alerting

📁 **`sentinel-alert/`**

### Why This Exists

Even with strong guardrails, **security drift still happens**:
- network exposure
- policy exemptions
- privilege escalation

This pillar detects **unsafe changes to the security posture itself**, not application-level noise.

### What This Pillar Demonstrates

- High-signal control-plane detections
- AzureActivity as authoritative telemetry
- Drift detection over alert spam
- Evidence-backed Sentinel analytics rules

Detections include:
- NSG internet exposure
- Policy exemption creation
- Subscription-scope RBAC privilege escalation

Each detection is backed by **query proof, rule configuration, and incident evidence**.

---

# Pillar 3 — Reactive & Enforced Security Response  
## Incident Playbooks (Sentinel SOAR)

📁 **`incident-playbooks/`**

### Why This Exists

Detection alone is insufficient in regulated environments.

Security teams must be able to **safely enforce response** without:
- breaking production
- creating audit gaps
- escalating blast radius

This pillar focuses on **enforcement-grade SOAR**, not auto-close playbooks.

### What This Pillar Demonstrates

- Sentinel analytics → automation → enforcement
- Logic App playbooks with explicit guardrails
- Human-in-the-loop vs automatic response decisions
- Least-privilege IAM modeling for automation
- Evidence-driven incident handling

### Included Projects

**Project 1 — RBAC Privilege Escalation**  
Tier-0 subscription role assignments with tag-based enforcement.

**Project 2 — Key Vault RBAC Containment**  
Contain secret access by removing vault-scope RBAC.

**Project 3 — Tier-0 CI/CD Secret Auto-Rotation**  
Automatic eradication of compromised DevOps secrets.

**Project 4 — Identity & Architecture Integrity Enforcement**  
Conditional Access, PIM, and landing zone drift enforcement.

This pillar demonstrates **platform security ownership**, not just incident handling.

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

This repository shows:
- how unsafe states are prevented
- how drift is detected
- how incidents are enforced
- how systems withstand audits

---

## What This Repository Says About the Engineer

A reviewer should conclude:

- This engineer understands Azure deeply (control plane, identity, policy)
- This engineer designs **preventative controls**, not just alerts
- This engineer builds SOAR that is safe, reversible, and auditable
- This engineer can operate in regulated, high-impact environments
- This engineer documents failures honestly and learns from them

---

## Final Note

This repository is intentionally structured as a **security architecture portfolio**, not a code dump.

Every folder exists to answer one question:

> **“Can this person be trusted to protect production in a regulated cloud?”**
