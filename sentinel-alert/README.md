# Sentinel Alerting & Detection Layer  
## Landing Zone Drift & Privileged Activity Monitoring (NDA-Safe)

This folder represents the **detection layer** of a regulated Azure landing-zone security architecture.

It contains **Microsoft Sentinel analytics rules** designed to detect:
- architecture drift
- guardrail bypass
- privilege escalation

These detections are intentionally **control-plane focused** and rely on **authoritative Azure telemetry**, not noisy workload signals.

This folder should be read in the context of the broader repository:
- `landing-zone-architecture/` defines the foundation
- `devops-change-control/` prevents unsafe changes
- `sentinel-alert/` detects when prevention is bypassed or misused
- `incident-playbooks/` enforces response

---

## What This Folder Is (and Is Not)

### This folder **IS**:
- A curated set of **high-signal Sentinel detections**
- Focused on **platform security**, not application telemetry
- Designed for **regulated environments** (aerospace / industrial)
- Backed by **evidence**, not assumptions

### This folder **IS NOT**:
- A generic SOC rule pack
- A collection of MITRE ATT&CK demos
- Alert spam based on VM or app logs

---

## Design Philosophy

These detections are built around a few core principles:

- **Control-plane telemetry first**  
  (AzureActivity, policy, RBAC, network configuration)

- **Drift detection over attack signatures**  
  Detecting *unsafe state changes*, not just exploitation

- **Architecture-aware alerts**  
  Alerts are meaningful only because a secure baseline exists

- **Audit-friendly by design**  
  Every detection is backed by screenshots and query proof

---

## Folder Structure
sentinel-alert/
├── detections/
│ ├── *.md # Detection design & KQL explanation
│ └── security-evidence/
│ ├── screenshots/
│ └── evidence indexes
└── README.md


---

## How This Fits the Architecture

The detections in this folder assume:

- A **central Log Analytics Workspace** owned by security
- AzureActivity flowing from all subscriptions
- Azure Policy enforcing baseline guardrails
- Clear subscription and identity boundaries

Without that foundation, these alerts would be:
- noisy
- incomplete
- easy to bypass

With it, they become **reliable security controls**.

---

## Intended Audience

This folder is written for:
- cloud security engineers
- security architects
- regulated-environment reviewers
- hiring managers evaluating real-world Azure security experience

The goal is not to impress with volume —  
it is to demonstrate **judgment, scope, and correctness**.

