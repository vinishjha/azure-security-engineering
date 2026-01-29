# Design Decisions & Lessons Learned

## Why AzureActivity Logs Were Used

Sentinel incidents and alerts do **not reliably expose `roleAssignmentId`**.

AzureActivity logs contain:
- `Microsoft.Authorization/roleAssignments/write`
- Full resource scope
- RoleAssignment GUID

This made AzureActivity the **authoritative source** for remediation.

---

## Why Human-in-the-Loop Was Required

Automatic RBAC removal without review is dangerous.

The tag-based approval model:
- Keeps humans in control
- Allows automation without risk
- Matches enterprise SOC workflows

---

## Why Managed Identity (Not Secrets)

- No credential storage
- No rotation overhead
- Clear audit attribution
- Enterprise-aligned security posture

---

## Why Two Permission Scopes Were Required

### Log Analytics Permissions
Used for:
- Querying AzureActivity logs
- Reading Sentinel incident data

### Subscription RBAC Permissions
Used for:
- Deleting role assignments
- Control-plane remediation

These are **different authorization planes** and must be handled separately.

---

## What Was Explicitly Avoided

- Blind auto-remediation
- Hard-coded roleAssignment IDs
- Alert JSON scraping
- Secret-based authentication
- Excessive debug comments

---

## Key Takeaway

> Detection ≠ Remediation  
> Visibility ≠ Authority  

Both must be designed intentionally.
