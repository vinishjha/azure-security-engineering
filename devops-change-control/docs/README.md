# Documentation Overview

This folder explains **why** the controls exist and **how** they were implemented.

Each document captures design intent, not just configuration steps.

---

## Documents

### 00-overview.md
Explains why DevOps pipelines are a **privileged security boundary** and why approvals alone are insufficient.

### 01-bicep-guardrail.md
Explains why **deny-based Azure Policy** is required instead of audit or remediation.

### 02-ado-pipeline-approval.md
Explains how Azure DevOps manual approvals enforce separation of duties but do not authorize insecure changes.

### 03-approval-telemetry-attempt.md
Documents the attempt to emit approval telemetry into Log Analytics, including real-world ingestion failures and lessons learned.

---

## What This Achieves

- Preserves security design reasoning
- Documents trade-offs and limitations
- Demonstrates engineering maturity
