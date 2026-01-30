# Validation & Proof

## Test Methodology

1. Created test service principal
2. Assigned Key Vault Secrets User role
3. Authenticated using CLI
4. Accessed Key Vault secret
5. Confirmed:
   - Audit logs generated
   - Sentinel incident created
   - Playbook revoked access

---

## Proof Screenshots

- `11-keyvault-role-created-deleted.png`
- `12-incident-creation-playbook-execution.png`

These show:
- Who created the role
- Who deleted it
- Timing correlation
