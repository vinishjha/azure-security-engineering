## Detection Example: RBAC Privilege Escalation – Subscription Role Assignment

### Detection Objective

Detect creation or deletion of Azure RBAC role assignments at **subscription scope**, which represents one of the highest-impact privilege changes in Azure environments.

### Architectural Context

- Identity is treated as the primary security perimeter
- Subscription-scope RBAC changes can immediately alter blast radius
- Privileged access changes must be visible, reviewable, and auditable

This detection aligns with:
- Landing zone governance boundaries
- Centralized logging architecture
- Preventive guardrail strategy with detective oversight

### Signal Source

- AzureActivity logs
- Operations:
  - `MICROSOFT.AUTHORIZATION/ROLEASSIGNMENTS/WRITE`
  - `MICROSOFT.AUTHORIZATION/ROLEASSIGNMENTS/DELETE`

### Detection Logic (High Level)

The analytic rule triggers when:
- A successful RBAC role assignment change occurs
- The target scope is the subscription (not resource group–scoped)
- The operation represents privilege grant or removal

Both assignment and rollback actions are captured to preserve audit history.

### Outcome

- A Sentinel alert is generated
- The alert is promoted to an incident
- The incident contains:
  - Caller identity
  - Subscription scope
  - Correlation ID
  - Timestamped activity

### Evidence

- Analytic rule configuration:  
  `security-evidence/screenshots/09-rbac-escalation-rule.png`

- AzureActivity query results:  
  `security-evidence/screenshots/10-rbac-assignment-query.png`

- Sentinel incident details:  
  `security-evidence/screenshots/11-rbac-assignment-incident.jpeg`

### Why This Matters

RBAC role assignment changes are a common source of:
- unintended privilege escalation
- emergency access grants
- audit findings in regulated environments

This detection ensures privileged access changes cannot occur silently.
