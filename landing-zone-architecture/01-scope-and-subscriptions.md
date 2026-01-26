## Subscription Model

### Platform Security Subscription
- `sub-platform-security`
- Hosts:
  - Log Analytics workspaces
  - Sentinel
  - Data Collection Rules
  - Security automation (Functions)

### Workload Subscription
- `sub-prod-workload`
- Hosts:
  - Application workloads
  - Network Security Groups
  - Network Watcher

## Rationale

Security controls are **centralized**.
Workloads cannot disable or bypass platform logging.
