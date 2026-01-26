# Landing Zone Overview

## Objective

Design a **predictable, auditable Azure foundation** where security is enforced by **architecture**, not manual processes.

This landing zone supports regulated workloads with strong separation of duties and centralized security oversight.

## High-Level Design

- `sub-platform-security`
  - Central logging (Log Analytics, Sentinel)
  - Security automation
  - Evidence retention

- `sub-prod-workload`
  - Regulated workloads
  - Network-isolated by default
  - No direct administrative ownership of security controls

## Design Principles

- Guardrails before alerts
- Identity is the primary control plane
- Default deny at scale
- Centralized evidence
- Explicit blast-radius control

## What This Enables

- Safe workload isolation
- Central detection and response
- Policy-driven prevention
- Auditable security posture
