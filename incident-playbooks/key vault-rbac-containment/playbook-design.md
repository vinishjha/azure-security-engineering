
---

# 📄 

```md
# Logic App Design — Key Vault Containment

## Trigger

Microsoft Sentinel Incident

---

## Steps

1. **Run Query (Log Analytics)**
   - Identify caller and vault

2. **Compose PrincipalOID**
   - Stabilizes expressions

3. **Compose VaultResourceId**
   - Determines RBAC scope

4. **HTTP: GetRoleAssignments**
   - ARM API call at Key Vault scope

5. **Filter Array**
   - Only assignments for suspicious identity

6. **Condition**
   - Exit safely if no RBAC found

7. **ForEach Delete**
   - Remove only those assignments

8. **Add Incident Comment**
   - SOC audit trail

---

## Screenshot Reference

- `10-playbook-design.png`
