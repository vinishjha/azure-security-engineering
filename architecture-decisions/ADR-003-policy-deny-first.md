# ADR-003: Preventive Guardrails Over Detective Controls

## Context

Detection alone is insufficient in regulated environments where:
- misconfigurations can create immediate exposure
- audit findings require proof of enforcement
- response time may not be acceptable

Preventing unsafe states is preferable to detecting them after the fact.

## Decision

Preventive controls are prioritized through:
- Azure Policy deny assignments
- DevOps approval gates
- Default-deny configurations

Detective controls (alerts) are used to:
- identify attempted violations
- detect drift or bypass attempts
- support investigation and audit

## Alternatives Considered

### 1. Detect-only approach
Rejected due to:
- delayed response
- higher operational risk
- weaker audit posture

### 2. Manual review processes
Rejected due to:
- lack of scalability
- inconsistency
- human error

## Consequences

### Positive
- Unsafe configurations blocked by default
- Reduced incident frequency
- Strong compliance posture

### Tradeoffs
- Requires upfront design effort
- Poorly designed policies can slow engineering

## Validation

- Policy denies observed during deployment
- Pipeline failures captured as evidence
- Sentinel alerts detect attempted violations
