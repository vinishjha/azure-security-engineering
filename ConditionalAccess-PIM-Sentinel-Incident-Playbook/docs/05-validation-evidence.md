## Evidence checklist

### Identity controls
- Entra diagnostic settings -> LAW (`entra-id-audit-signin-sentinel`)
- Conditional Access policies (Tier-0 + baseline)
- PIM eligible role + activation settings

### Detections
- Analytics rule configuration screenshots (name/severity/query)
- Incidents for:
  - CA drift
  - PIM activation
  - Permanent privileged role assignment

### SOAR
- Logic App designer (trigger + actions)
- Successful run history
- Incident shows automated comment + tags
