# Detection Evidence  
## Audit Proof for Sentinel Alerting

This folder contains **evidence artifacts** proving that the Sentinel detections in this project:

- exist
- are enabled
- receive telemetry
- trigger incidents
- align with the intended architecture

These screenshots are organized intentionally to mirror how **regulated audits** are conducted.

---

## Evidence Organization

### Platform Foundations

These screenshots establish that the detection environment itself is valid.

- **01 – Subscription boundary proof**  
  Demonstrates separation between platform/security and workload subscriptions.

- **02 – Central LAW ownership proof**  
  Shows the Log Analytics Workspace is owned by the security platform subscription.

- **03 – AzureActivity telemetry proof**  
  Confirms control-plane activity is ingested into the central workspace.

---

### Detection Configuration

- **04 – Alert rules**  
  Shows the Sentinel analytics rules enabled:
  - NSG Internet Exposure
  - Policy Guardrail Bypass
  - RBAC Privilege Escalation

This proves the detections are not drafts or disabled rules.

---

### Detection Execution & Incidents

- **05 – NSG risky rules**  
  Example incident generated from an NSG exposure event.

- **06 – Incident events**  
  Underlying AzureActivity events that caused the incident.

- **07 – Policy exemption created**  
  Evidence of a policy exemption operation in AzureActivity.

- **08 – Policy exemption incident**  
  Sentinel incident created from the exemption detection.

- **09 – RBAC escalation rule**  
  Enabled RBAC privilege escalation analytics rule.

- **10 – RBAC assignment query**  
  Query results showing role assignment write activity.

- **11 – RBAC assignment incident**  
  Sentinel incident created from subscription-scope role assignment.

---

## Why This Evidence Exists

In regulated environments, security controls must be:
- provable
- reviewable
- repeatable

Screenshots are used here not as decoration, but as **audit artifacts** that demonstrate:
- telemetry flow
- detection correctness
- incident creation
- architectural alignment

---

## How Reviewers Should Use This Folder

Reviewers should:
1. Read the detection design (`detections/*.md`)
2. Validate the platform foundation screenshots
3. Validate the alert configuration
4. Validate at least one incident per detection

This mirrors real audit and security review workflows.
