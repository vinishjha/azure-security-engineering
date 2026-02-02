# DevOps Change Control – Enforced Security Architecture (Azure)

## Purpose

This project demonstrates a **real-world DevOps change control architecture** for **regulated Azure environments**, with a focus on **preventative security controls**, **human approval gates**, and **centralized audit evidence**.

The objective is not simply to deploy infrastructure, but to **prove that insecure changes cannot reach production**, even when approved by humans.

This repository intentionally includes **expected failures** (policy denials, blocked deployments, ingestion errors) to demonstrate **control effectiveness**, not just successful deployments.

---

## Security Questions This Project Answers

1. How do we prevent insecure infrastructure changes from reaching production?
2. How do we enforce security even after human approval?
3. How do we record approvals and enforcement outcomes outside CI/CD?
4. How do we turn governance violations into security detections?

---

## High-Level Change Flow

1. Developer triggers Azure DevOps pipeline
2. Pipeline performs **What-If (advisory)**
3. **Manual approval** required for production
4. Deployment executes under **Azure Policy enforcement**
5. Non-compliant resources are **explicitly denied**
6. Enforcement events are captured for **audit and detection**

Approval ≠ Authorization.

---

## Technologies Used

- Azure DevOps Pipelines (YAML)
- Azure Bicep (IaC)
- Azure Policy (deny-based guardrails)
- Azure Monitor / Log Analytics
- Data Collection Endpoints (DCE)
- Data Collection Rules (DCR)
- Microsoft Sentinel
- Microsoft Defender for Cloud

---

## Repository Structure
devops-change-control/
├── architecture/ # Security architecture & threat modeling
├── bicep/ # Infrastructure under governance
├── policies/ # Azure Policy deny guardrails
├── pipelines/ # Azure DevOps pipeline YAML
├── docs/ # Design and implementation docs
├── evidence/ # Screenshot-based proof
├── sentinel/ # Detection logic (KQL)
├── defender/ # Incident triage narratives
├── screenshots2/ # Raw supporting screenshots
└── README.md


---

## Why This Matters in Aerospace & Regulated Environments

Aerospace environments require:

- Preventative controls (not detective-only)
- Separation of duties
- Immutable audit trails
- Proven enforcement, not policy intent

This architecture ensures:

- CI/CD cannot bypass governance
- Approvals do not override security
- Every denied change is auditable
- Governance violations become SOC-visible signals

This aligns with NIST 800-53, NIST 800-171, DFARS, CMMC, and internal airworthiness controls.

---

## Audience

- Azure Security Engineers
- Cloud Security Architects
- DevSecOps Engineers
- Aerospace / Defense security teams
