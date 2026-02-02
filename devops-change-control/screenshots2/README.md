# screenshots2 — Raw Evidence (DevOps Change Control)

This folder contains the **raw, original screenshots** used as evidence for the DevOps Change Control project.

These images are intentionally kept **unchanged** (no cropping/reformatting) so they can serve as credible proof during:
- interviews / portfolio reviews
- security design reviews
- compliance/audit discussions

The screenshots document the full story: **approval gate → enforcement → audit telemetry plumbing → real-world troubleshooting**.

---

## Evidence Map (by filename)

### 1) `ado-manual-approval.png`
**What you’re seeing**
- Azure DevOps pipeline run `#20260125.1 - Update azure-pipelines.yml`
- Stage flow displayed visually:
  - ✅ **What-If (advisory)**
  - ✅ **Approval Gate (prod)** (manual validation passed)
  - ❌ **Deploy (prod - enforced)** (failed)
- Error summary indicates a script failure tied to the approval marker / logging step.

**Why it matters in this project**
- Proves a *human-in-the-loop* control exists: **production requires explicit approval**
- Demonstrates the intended separation-of-duties pattern:
  - Devs can propose changes
  - A designated approver must authorize “go/no-go”
- Shows the pipeline is built like a real regulated workflow (what-if + approval + enforced deploy)

**Security control demonstrated**
- Change governance + accountability: *who approved what, when*

---

### 2) `policy-assignment-deny-storage.png`
**What you’re seeing**
- Azure Policy Assignment named **`Deny-Storage-Public-Exposure`**
- Scope includes **`sub-prod-workload/rg-prod-test`** (workload scope, not random)
- Description indicates the policy prevents public exposure.

**Why it matters in this project**
- This is the **enforcement backbone**: even if CI/CD attempts a risky change, policy denies it.
- Demonstrates a key principle of regulated environments:
  - **approvals do not override guardrails**
  - “approved” is not the same as “allowed”

**Security control demonstrated**
- Preventative governance: **deny-based guardrail** against data exposure risk

---

### 3) `custom-table-devopsapprovals-exists.png`
**What you’re seeing**
- Azure CLI output confirming Log Analytics custom table exists:
  - `DevOpsApprovals_CL`
  - provisioning state: **Succeeded**
- Command shown: `az monitor log-analytics workspace table list ...`

**Why it matters in this project**
- Proves you created a dedicated **audit sink** outside Azure DevOps.
- This is important because pipeline logs alone aren’t a durable compliance record:
  - retention limits
  - access controls differ
  - auditors often want evidence in a central logging platform

**Security control demonstrated**
- Audit trail readiness: a dedicated table for “approval markers”

---

### 4) `dce-created.png`
**What you’re seeing**
- Azure Portal page for Data Collection Endpoint (DCE) **`dce-devops-audit`**
- Status: **Succeeded**
- Shows the two critical endpoints:
  - Configuration access endpoint
  - Logs ingestion endpoint (regional ingest URL)

**Why it matters in this project**
- Confirms you’re using **modern Azure Monitor ingestion** (DCE/DCR pipeline), not legacy collectors.
- This is the plumbing required to ingest structured approval events (custom logs) securely.

**Security control demonstrated**
- Centralized telemetry infrastructure (foundation for controlled ingestion)

---

### 5) `dcr-created-immutableid.png`
**What you’re seeing**
- Azure Portal page for Data Collection Rule (DCR) **`dcr-devops-approvals`**
- Shows an **Immutable ID**
- Shows the linked Data Collection Endpoint: **`dce-devops-audit`**

**Why it matters in this project**
- Proves the DCR is present and anchored by an immutable identifier.
- In regulated environments, immutability supports integrity of ingestion configuration:
  - traceability
  - change control around telemetry pipelines

**Security control demonstrated**
- Controlled ingestion routing: “this stream goes to this destination under this rule”

---

### 6) `pipeline-ingestion-403.png`
**What you’re seeing**
- Azure DevOps job logs for step:
  - **“Emit Approval Marker to Log Analytics (DevOpsApprovals_CL)”**
- The script attempts to POST approval telemetry to:
  - `https://<dce>.ingest.monitor.azure.com/.../streams/Custom-DevOpsApprovals_CL`
- Failure shown clearly:
  - `Ingestion HTTP status: 403`
  - `Expected 204 from ingestion API, got 403`
  - Script exits with code 1

**Why it matters in this project**
- This screenshot proves two important things:
  1) The pipeline is correctly attempting to produce **approval audit telemetry**
  2) Real-world ingestion requires correct identity + permissions (RBAC / token audience / scope)

**Security control demonstrated**
- Troubleshooting maturity: documenting an auth failure is part of real security engineering.
- In aerospace, capturing and fixing “403 ingestion” is not optional — telemetry integrity is mission-critical.

---

### 7) `function-deploy-trigger-sync-failure.png`
**What you’re seeing**
- A deployment sequence uploading a Function release ZIP to blob storage (PUT)
- Response indicates upload succeeded:
  - `Response status: 201`
- Deployment then fails at:
  - **“Syncing Triggers…”**
  - `Operation returned an invalid status 'Bad Request'`

**Why it matters in this project**
- This indicates a supporting automation component (Function App) exists in the ecosystem (commonly used to emit/log events or act as an ingestion helper).
- It captures a realistic operational edge case:
  - artifact upload succeeds
  - runtime trigger synchronization fails
- This is common in real cloud operations and often relates to:
  - app settings / runtime config drift
  - permissions needed by deployment user
  - platform state requiring restart/redeploy

**Security control demonstrated**
- Operational realism: you can build controls, deploy them, and face real deployment failures—then troubleshoot them.

---

## How these screenshots fit together (the narrative)

1. **Change enters pipeline** → `ado-manual-approval.png`
2. **Guardrail is in place** → `policy-assignment-deny-storage.png`
3. **Audit sink exists** → `custom-table-devopsapprovals-exists.png`
4. **Ingestion plumbing exists** → `dce-created.png` + `dcr-created-immutableid.png`
5. **Pipeline tries to emit approval telemetry** → `pipeline-ingestion-403.png`
6. **Supporting automation has real deployment edge cases** → `function-deploy-trigger-sync-failure.png`

This is the complete “security engineering story”:
- governance + enforcement + auditability + operational troubleshooting

---

## Why aerospace teams care about this exact evidence

Aerospace environments typically require:
- separation of duties (approval gates)
- preventative controls (deny-based policies)
- immutable / centralized audit evidence (Log Analytics / Sentinel)
- reliable telemetry pipelines (DCE/DCR ingestion)
- documented troubleshooting steps (403s and deployment edge cases are real life)

These screenshots are not cosmetic — they are **control validation artifacts**.
