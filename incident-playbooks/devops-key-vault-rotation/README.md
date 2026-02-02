# 🔐 Tier-0 CI/CD Secret Auto-Rotation (Key Vault + Sentinel + Logic Apps)

In this project, I built an end-to-end **Tier-0 secret protection** workflow in Azure that automatically responds to suspicious Key Vault secret access.

**What I implemented:**
- A CI/CD pipeline reads a Key Vault secret (expected behavior).
- Microsoft Sentinel detects Key Vault `SecretGet` events for a Tier-0 secret (detection).
- A Logic App playbook automatically performs **containment**, including disabling older secret versions and writing evidence back into the incident.

My goal was **fast containment + audit-quality documentation**, not manual response.

---

## Why I built this

In real enterprises, CI/CD secrets can effectively become **Tier-0 credentials** because they can unlock:
- deployment pipelines,
- privileged automation,
- access to regulated production environments.

If a Tier-0 secret is accessed unexpectedly, I want a response that:
1) reduces attacker reuse time,
2) produces evidence automatically,
3) is repeatable and safe.

---

## What I built (end-to-end)

### A) I created the Tier-0 secret in Key Vault
I created the secret used by the CI/CD pipeline and added tags so it is clearly classified and governed as a Tier-0 CI/CD secret.

📸 `screenshots/00-keyvault-name-secret-name.png`

---

### B) I created a Service Principal (App Registration) for CI/CD
I created an app registration / service principal to represent the pipeline identity. This gives my pipeline a dedicated, auditable identity that can be monitored and controlled.

📸 `screenshots/01-app-registration-service-principal.png`

---

### C) I configured the Azure DevOps service connection
I created an Azure Resource Manager service connection in Azure DevOps using that service principal. This is the pipeline’s authenticated “path” into Azure.

📸 `screenshots/02-service-principal-devops.png`

---

### D) I validated the pipeline can retrieve the secret
I ran the pipeline and confirmed the secret retrieval succeeds. This step proves the CI/CD flow is real and generates real Key Vault audit events to detect.

📸 `screenshots/03-cicd-secret-received.png`

---

### E) I created the Sentinel analytics rule
I created a custom analytics rule to detect `SecretGet` events for my Tier-0 secret and generate incidents for investigation and response automation.

📸 `screenshots/04-analytics-rule-created.png`

---

### F) I validated the KQL and the event fields
I validated the query output and ensured the fields needed for automation (like requestUri and identity values) are present and usable.

📸 `screenshots/05-kql-query.png`

---

### G) I confirmed incident creation and detection behavior
I confirmed the analytics rule creates incidents and that those incidents show up as expected for automated response.

📸 `screenshots/06-incident-creation.png`  
📸 `screenshots/07-incident-detected.png`

---

### H) I built the Logic App playbook for containment + documentation
I built the playbook to:
- enrich incident context (vault name, secret name, caller identity),
- list secret versions,
- identify versions to disable,
- disable old secret versions safely,
- add an incident comment and update the incident state/tags.

📸 `screenshots/08-playbook-design-1.png`  
📸 `screenshots/09-playbook-design-2.png`

---

## Repository contents

- [`architecture.md`](architecture.md) – how the solution is wired and why
- [`detection.md`](detection.md) – analytics rule + KQL strategy
- [`playbook-design.md`](playbook-design.md) – every Logic App step explained
- [`permissions.md`](permissions.md) – roles/RBAC and why they exist
- [`validation.md`](validation.md) – how I tested end-to-end
- [`aerospace-rationale.md`](aerospace-rationale.md) – why this matters in aerospace/regulated environments
- [`lessons-learned.md`](lessons-learned.md) – what I learned while building it

---

## Safety notes

- I never store secret values in GitHub.
- I treat Tier-0 response as a controlled change: logged, documented, and reviewable.
- Disabling old versions preserves audit history while invalidating old credentials.
