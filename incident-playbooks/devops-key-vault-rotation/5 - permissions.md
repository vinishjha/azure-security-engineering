# Permissions & RBAC

This document explains the permissions I assigned and why they are needed.

---

## Identities I used

### 1) CI/CD identity (Azure DevOps service principal)
I used a dedicated service principal for the pipeline so secret access is:
- attributable (identity shows up in logs),
- controllable (permissions can be tightened),
- auditable (clear separation from human accounts).

### 2) Response identity (Logic App managed identity)
I used the Logic App managed identity to perform response actions so:
- the playbook does not need embedded credentials,
- access can be scoped and audited through RBAC.

---

## Key Vault permissions

### CI/CD identity
I granted the minimum required permissions for the pipeline to read the secret (for normal operation).

### Playbook identity
I granted the playbook identity permission to:
- list secret versions,
- disable secret versions (PATCH attributes).

This is required because containment is implemented by disabling old versions.

---

## Sentinel / incident update permissions

I granted the playbook identity the ability to:
- add incident comments,
- update incident status/tags.

This is required so the playbook creates an evidence trail directly inside Sentinel.

---

## Why I separated CI/CD and response permissions

Architecturally, separation prevents:
- pipelines from having incident-control permissions,
- responders from needing CI/CD-level credentials.

It also supports least privilege and clean audit boundaries.
