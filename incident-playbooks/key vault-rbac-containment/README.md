# 🔐 Key Vault RBAC Containment Playbook

## Overview

This project demonstrates an automated containment workflow in Microsoft Sentinel that **revokes Key Vault RBAC permissions** after suspicious secret access is detected.

The goal is **least-privilege containment**, not destructive remediation.

No secrets are deleted.
No identities are removed.
Only RBAC permissions enabling the access are revoked.

---

## What This Project Solves

Key Vault audit events often indicate **successful secret access**, but:

- Sentinel incidents do not reliably expose *who* accessed the secret
- RBAC role assignments enabling that access are not surfaced directly
- Manual investigation delays containment

This playbook closes that gap.

---

## High-Level Flow

1. Key Vault secret is accessed successfully
2. Azure Diagnostics logs capture caller identity
3. Sentinel analytic rule raises an incident
4. Analyst approves containment via incident tag
5. Logic App:
   - Identifies the caller
   - Finds Key Vault RBAC assignments
   - Removes only the caller’s permissions
6. Action is documented in the incident timeline

---

## Key Capabilities Demonstrated

- Microsoft Sentinel detection engineering
- Azure Diagnostics → Log Analytics ingestion
- KQL-based identity attribution
- ARM REST API RBAC control
- Managed Identity authorization
- Human-in-the-loop containment
- SOC-ready audit documentation

---

## Architecture

See [architecture.md](architecture.md)

---

## Screenshots

All screenshots referenced below are stored in `screenshots/` and mapped one-to-one with documentation sections.

---

## Status

✅ Implemented  
✅ Tested with service principal  
✅ Production-grade containment logic  
