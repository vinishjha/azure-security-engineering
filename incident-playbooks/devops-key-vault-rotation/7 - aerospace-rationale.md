# Why I built this for Aerospace / Regulated Environments

In aerospace and other regulated industries, CI/CD secrets are rarely “just secrets.”
They often protect automation that can touch:
- production workloads,
- engineering systems,
- regulated data boundaries,
- critical infrastructure changes.

I treat CI/CD secrets as **Tier-0** when compromise could enable:
- privileged deployments,
- infrastructure modification,
- lateral movement across subscriptions,
- silent persistence via automation identities.

---

## Situations where I would use this pattern

### 1) Compromised pipeline identity
If the service connection credential is stolen, an attacker can use it outside the pipeline.
This solution detects secret reads and rapidly invalidates older versions to reduce reuse.

### 2) Insider or unexpected access
If a human identity reads a CI/CD secret, I want immediate containment and full evidence capture.

### 3) Compromised build agent
If a build agent is compromised, secrets pulled during the run can be exfiltrated.
Automated containment reduces dwell time.

### 4) Supply-chain or repo compromise
If pipeline changes cause secrets to be accessed unusually, I want detection + automated response.

---

## Why automation matters in regulated environments

I built this to achieve:
- **Speed**: containment in seconds/minutes, not hours
- **Consistency**: the same response every time
- **Auditability**: incident comments and updates become evidence

---

## How I would extend this in an enterprise program

### Prevent
- Azure Policy to enforce Key Vault logging and secure configuration
- Least privilege RBAC for Key Vault and pipeline identities
- Private networking controls for Key Vault where applicable

### Detect
- Additional analytics rules for abnormal patterns (spike, new IP, new identity)
- Correlation with RBAC changes, DevOps pipeline edits, or new service principal creation

### Respond
- Disable service principal credentials when compromise is suspected
- Quarantine pipeline runs and require approvals
- Notify IR via Teams/Email/ITSM and open a change ticket with evidence attached

---

## Bottom line

I built a Tier-0 response pattern that is fast, repeatable, and audit-friendly.
That is exactly what regulated environments need when CI/CD credentials are at risk.
