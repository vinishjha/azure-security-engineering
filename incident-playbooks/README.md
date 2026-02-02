# Incident Playbooks — Sentinel SOAR + Enforcement (NDA-Safe Portfolio)

This folder contains **portfolio-grade, NDA-safe incident playbooks** built with **Microsoft Sentinel**, **Azure Logic Apps**, **Log Analytics**, **Azure RBAC/ARM APIs**, **Key Vault**, **Azure DevOps**, and (in Project 4) **Entra ID Conditional Access + PIM**.

These are **not “toy” playbooks** that send email or close incidents.

Each subfolder is a complete project that demonstrates how a regulated enterprise (aerospace/industrial/critical infrastructure) designs:

- **Detection** (high-signal KQL)
- **Incident workflows** (automation rules and lifecycle controls)
- **Enforcement** (safe, reversible containment actions)
- **Auditability** (incident comments, Activity Log proof, evidence screenshots)
- **Engineering maturity** (idempotency, loop prevention, least privilege, explicit tradeoffs)

---

## How to Read This Folder

Each project follows the same engineering narrative:

1. **Threat model / control objective**
2. **Telemetry source(s)** and why they’re authoritative
3. **KQL detection** → **Sentinel analytics rule**
4. **Incident creation + automation rule**
5. **Logic App playbook** design (branches, expressions, failure modes)
6. **Permissions model** (why the playbook identity needs specific roles)
7. **Testing methodology** (how we proved it works)
8. **Evidence** (screenshots + incident timelines)
9. **Lessons learned** (what failed, what we changed, and why)

You can review any project independently, but together they show a progression:

**Object cleanup (Project 1) → Data access containment (Project 2) → Tier-0 eradication (Project 3) → Identity + architecture integrity enforcement (Project 4)**

---

## Common Architecture Pattern (All Projects)

At a high level, all projects implement:

Control Plane / Data Plane / Identity Signals
↓
Log Analytics Workspace (LAW)
↓
Sentinel Analytics Rule (KQL)
↓
Incident
↓
Sentinel Automation Rule (Trigger)
↓
Logic App Playbook (Managed Identity)
↓
Enforce Safe State + Add Incident Audit Comment


### Design principles used throughout
- **Control-plane signals first** (AzureActivity, AuditEvent)
- **High signal > high volume**
- **Drift detection** over alert spam
- **Managed Identity over secrets**
- **Fail-safe automation** (guardrails, conditions, idempotency)
- **Evidence before claims** (every project includes proof)

---

# Projects

---

## Project 1 — RBAC Privilege Escalation: Subscription Role Assignment Change

📁 `project-01-rbac-privilege-escalation/` (folder name may vary)

### Objective
Detect and contain **privileged RBAC role assignment changes** at subscription scope, specifically:
- **Owner**
- **Contributor**
- **User Access Administrator**

These changes are Tier-0 because they can immediately enable:
- full resource takeover
- policy bypass
- logging suppression
- persistence

### What we built (end-to-end)

#### 1) Telemetry and signal selection
- Chose **AzureActivity** as the authoritative control-plane source.
- Confirmed activity logging is flowing into the **central LAW** and Sentinel.

**Why:** Sentinel incident payloads often lack roleAssignmentId. AzureActivity reliably contains it in the ResourceId / Properties.

#### 2) KQL analytics rule (detection)
- Built a scheduled analytics rule detecting:
  - `Microsoft.Authorization/roleAssignments/write`
- Tuned to prioritize *privileged* roles at subscription scope.

#### 3) Sentinel incident workflow
- Incident created from analytics rule
- **Human-in-the-loop** control via tag:
  - `RBAC-AutoRemove`

**Why tag-based:** RBAC removals can disrupt legitimate work. In regulated environments, privilege removal often requires an analyst decision.

#### 4) Logic App playbook (remediation)
- Triggered by incident update when tag is present.
- Steps:
  1. **Get Incident** (V3) → read labels/tags + incident context
  2. **Run Log Analytics Query** → find the role assignment write event in the incident time window
  3. Extract roleAssignment GUID from AzureActivity ResourceId/Properties
  4. **HTTP DELETE** to ARM:
     - `/providers/Microsoft.Authorization/roleAssignments/{guid}`
  5. **Add comment to incident** documenting what was removed and why

#### 5) Permissions model (critical lesson)
To work end-to-end, the playbook identity needed *two different authorization domains*:

- **Microsoft Sentinel Contributor** at the workspace scope  
  → to read incidents, add comments, interact with Sentinel connectors
- **User Access Administrator** (or Owner) at subscription scope  
  → to actually delete role assignments

**Key principle:** *Sentinel data access ≠ control-plane enforcement permissions.*

#### 6) How we validated
- Created a test privileged assignment
- Confirmed incident creation
- Applied tag
- Verified playbook removed the assignment
- Captured Activity Log proof + incident timeline

### What this project proves
- RBAC threat modeling
- KQL detection engineering
- Human-in-the-loop SOAR design
- ARM API enforcement
- Least-privilege IAM reasoning
- Audit-grade evidence capture

---

## Project 2 — Key Vault RBAC Containment: Secret Access → Remove Vault-Scope RBAC

📁 `project-02-keyvault-rbac-containment/`

### Objective
Automatically contain suspicious Key Vault secret access by:
- Identifying **who accessed a secret**
- Identifying **RBAC assignments at the Key Vault scope** that enabled that access
- Removing those RBAC assignments (least-privilege containment)
- Documenting actions in the Sentinel incident

**This is not about deleting users or secrets.**  
It’s about revoking permissions at the **Key Vault scope**.

### What we built (end-to-end)

#### 1) Telemetry and signal selection
- Enabled Key Vault diagnostics to Log Analytics:
  - `AzureDiagnostics` → `Category == "AuditEvent"`
- Verified SecretGet events appear in LAW.

**Why:** Sentinel incident payloads don’t reliably expose caller identity / vault context for Key Vault operations. AuditEvent logs do.

#### 2) KQL analytics rule (detection)
Detection focused on successful secret access:
- `OperationName` in (`SecretGet`, `KeyGet`, `CertificateGet`)
- `ResultType == Success`
- Extracted:
  - `PrincipalOID` (who)
  - `VaultResourceId` (where)

#### 3) Sentinel incident workflow
- Incident created from analytics rule
- Automation rule triggers the playbook on incident creation/update (implementation may vary)

#### 4) Logic App playbook (containment)
Core steps:
1. Query LAW to get latest relevant SecretGet event:
   - `PrincipalOID`
   - `VaultResourceId`
2. Use ARM API to list role assignments at that vault scope:
   - `GET {VaultResourceId}/providers/Microsoft.Authorization/roleAssignments`
3. Filter assignments where `principalId == PrincipalOID`
4. If assignments found:
   - DELETE each role assignment
5. Add Sentinel comment:
   - who was contained
   - which assignments removed
   - time / reason

#### 5) Permissions model
- Needs Log Analytics read permissions to query audit logs
- Needs **User Access Administrator** (or Owner) to delete RBAC assignments at vault scope
- Needs Sentinel contributor to write incident comments

#### 6) Validation methodology
- Created a test Service Principal
- Assigned Key Vault Secrets User at vault scope
- Used CLI login as SP
- Accessed secret (generated AuditEvent)
- Confirmed incident + playbook execution
- Verified RBAC role removed
- Captured proof: “role created” and “role deleted”

### What this project proves
- Data-plane access detection (Key Vault)
- RBAC as the real attack path
- Automated containment with least-privilege logic
- Cross-domain engineering: Sentinel + LAW + ARM + identity
- Real-world troubleshooting of connector limitations + arrays + expressions

---

## Project 3 — Tier-0 CI/CD Secret Auto-Rotation: Sentinel + Logic Apps + Azure DevOps

📁 `project-03-tier0-cicd-secret-auto-rotation/`

### Objective
Protect a **Tier-0 CI/CD secret** used by an Azure DevOps pipeline.

If the secret is accessed, we automatically:
- identify which secret and which vault were involved
- enumerate secret versions safely (without reading secret values)
- disable the previously enabled secret version
- create/enable a new version
- add tags / rotation metadata
- document the response in the Sentinel incident

**This is eradication, not containment.**  
For CI/CD Tier-0 credentials, waiting for human approval can be too slow.

### What we built (end-to-end)

#### 1) Tier-0 classification model
We explicitly defined “auto-rotatable” secrets with tags like:
- `SecretTier = Tier-0`
- `RotationMode = Automatic`
- `Usage = CI-CD`
- `Owner = Security`

**Why:** automatic rotation must be defensible. Classification makes it safe and auditable.

#### 2) DevOps integration (real consumer)
- Created service principal and service connection
- Built a minimal pipeline that retrieves the secret from Key Vault
- Proved the consumer is real, not hypothetical

#### 3) Detection (KQL)
- Sentinel scheduled rule using Key Vault AuditEvent logs:
  - `OperationName == SecretGet`
  - extracted secret name from request URI
  - identity extraction using schema-safe patterns (e.g., `column_ifexists()`)

**Major pitfall we solved:** queries that work in Logs sometimes fail in Analytics Rule validation; we simplified to deterministic schemas for rules.

#### 4) The hardest problem: alert/rotation loops
Early versions caused loop conditions:
- playbook touches Key Vault → generates new SecretGet events → triggers rule → playbook runs again → endless rotations

We fixed this with engineering guardrails:
- avoid reading secret values
- use **List Versions** endpoints instead of SecretGet
- idempotency controls (rotation metadata, suppression)
- exclusions for automation identities when appropriate

#### 5) Playbook design (key decisions)
- Used “Compose” as explicit debug anchors
- Enumerated versions via REST:
  - `GET /secrets/{name}/versions`
- Filtered only enabled versions
- Avoided `sort()` on fields that may not exist
  - chose deterministic `first()` because API returns newest → oldest

#### 6) Outcomes and evidence
- Pipeline run → generated audit events
- Sentinel incident → created reliably
- Playbook run → disabled old version and renewed
- Incident comment recorded exact version disabled
- Activity log / Key Vault version views captured as evidence

### What this project proves
- Tier-0 thinking: CI/CD supply chain risk
- Real automation safety: loop prevention + idempotency
- Key Vault version lifecycle management
- Detection + response engineering across DevOps + Sentinel + Key Vault

---

## Project 4 — Hybrid Identity & Architecture Integrity Enforcement (CA + PIM + Landing Zone Guardrails)

📁 `project-04-identity-architecture-enforcement/`

### Objective
Protect the **security architecture itself**, not just a specific object.

Project 4 is a hybrid enforcement framework:
- **Tier-0 identity guardrails** are auto-restored immediately (no human delay)
- **Tier-1 identity guardrails** require a tag-based approval before enforcement
- **Landing zone drift** (policy/logging/network integrity) is automatically enforced

This project is designed to clearly signal:
> “This person owns platform security posture.”

### What this project will include (system-level)

#### A) Conditional Access (CA) drift detection + enforcement
Detect:
- CA policy disabled, weakened, or exclusions added
Enforce:
- Tier-0 CA policies: auto-restore baseline state
- Tier-1 CA policies: require tag (human approval) before restore

#### B) PIM drift / privilege misuse detection + enforcement
Detect:
- permanent privileged assignments
- activation policy weakened
- non-PIM privileged grants
Enforce:
- Tier-0 privileged roles: auto-expire / remove permanence
- Tier-1 roles: tag-based enforcement

#### C) Landing zone drift detection + enforcement (automatic)
Detect and enforce safe state for:
- policy exemptions created/updated (remove exemption)
- diagnostic settings removed (re-enable to central LAW)
- public exposure enabled (restore private-only posture)
- privileged drift for automation identities (remove broad roles)

#### D) Master playbook framework (single orchestrator)
A single master Logic App classifies incident type:
- CA drift
- PIM drift
- platform drift

Then applies enforcement:
- auto for Tier-0
- tag-based for Tier-1

All actions produce:
- incident comment audit trail
- activity log proof
- repeat-offender tracking (optional)

### Why this is the signature project
It demonstrates:
- identity-first thinking (CA + PIM)
- platform drift enforcement
- safety & governance in automation
- regulated environment posture ownership

---

## Evidence and Proof Expectations (Across All Projects)

Each project folder should contain:
- `docs/` explanations (why, what, how)
- `detections/` KQL
- `playbooks/` Logic App documentation
- `evidence/` screenshots for:
  - analytics rule
  - KQL query results
  - incident creation
  - automation rule
  - playbook design + run history
  - activity log proof of enforcement

This folder is deliberately designed so that a reviewer can confirm:
- the system exists
- it works end-to-end
- it is built with mature guardrails
- it is auditable and explainable

---

## Why This Folder Matters (Interview Narrative)

A hiring manager reviewing this folder should conclude:

- This engineer can build high-signal detections
- This engineer can automate enforcement safely
- This engineer understands Azure control plane and identity boundaries
- This engineer designs with regulated constraints (auditability, least privilege, failure safety)
- This engineer can own and defend security architecture decisions

---

## Disclaimer

This repository is for demonstration purposes only.  
It is not affiliated with any employer and contains no proprietary or confidential information.
