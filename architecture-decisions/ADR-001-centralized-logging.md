# ADR-001: Centralized Logging as a Platform Security Control

## Context

In a regulated Azure environment with multiple subscriptions, security visibility must be:
- consistent across workloads
- protected from workload-level tampering
- centrally accessible for detection, investigation, and audit

Decentralized logging at the subscription or resource level creates risk:
- logs can be disabled by workload owners
- retention policies can drift
- investigations become fragmented

## Decision

All security-relevant telemetry is centralized in a **platform-owned Log Analytics workspace** hosted in the `sub-platform-security` subscription.

Workload subscriptions do not own:
- log retention
- workspace configuration
- Sentinel enablement

Telemetry ingestion is enforced via:
- Azure diagnostic settings
- Data Collection Rules (DCRs)
- Platform-level ownership

## Alternatives Considered

### 1. Per-subscription workspaces
Rejected due to:
- inconsistent retention
- higher operational overhead
- increased risk of logging gaps

### 2. Resource-level logging only
Rejected due to:
- ease of tampering
- lack of centralized correlation
- audit complexity

## Consequences

### Positive
- Strong separation of duties
- Centralized detection and hunting
- Consistent evidence retention
- Reduced blast radius from workload compromise

### Tradeoffs
- Requires platform ownership maturity
- Central workspace becomes a critical dependency

## Validation

- AzureActivity logs observed centrally
- Diagnostic settings enforced
- Sentinel analytics run against centralized data
- Evidence indexed in `security-evidence/`
