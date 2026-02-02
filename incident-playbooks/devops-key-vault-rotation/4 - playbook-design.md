
---

# 4) `playbook-design.md` 

```md
# Playbook Design (PB-KV-Tier0-Secret-Response)

This document explains what I built inside the Logic App and why each step exists.

---

## Trigger

### Microsoft Sentinel incident trigger
I designed the playbook to start from a Sentinel incident so I can:
- use the incident as the control point for response,
- update the incident with evidence (comments/tags/status),
- ensure the response is traceable.

---

## Control gate

### Condition 1 (True/False branch)
I used a condition gate so the Tier-0 response runs only when the incident matches my intended scenario.
Architecturally, this prevents accidental automation against unrelated incidents.

---

## Enrichment

### Run query and list results (Log Analytics)
I re-query Log Analytics inside the playbook to get authoritative event context such as:
- requestUri (vault/secret/version)
- caller identity fields (OID/AppId/UPN)
- timestamps around the suspicious access

This ensures my playbook actions are driven by the event data, not assumptions.

---

## Parsing and normalization (Compose steps)

I used Compose actions to extract consistent values from log output:

### Compose PrincipalOID
I extract the caller identity (OID) so I can record who accessed the secret.

### Compose SecretName
I extract the secret name from the requestUri so the playbook can operate on the correct secret.

### Compose RequestUri
I normalize the request URI so subsequent parsing and URI construction is consistent.

### Compose VaultName
I extract the vault host name so I can build Key Vault REST URIs reliably.

---

## Secret version handling

### HTTP List secret versions
I call Key Vault to list all versions of the secret.
This is required because containment depends on version IDs.

### Filter array
I filter the returned versions to the set I intend to operate on (for example: enabled versions).

### Compose LatestEnabledVersionId
I identify the latest enabled secret version ID.
Architecturally, I do this so I can avoid disabling the newest version by accident.

### Compose LastAutoRotateUtc
I generate a timestamp that I later write into the incident so the response is auditable.

### Compose SecretVersion
I compute the specific version identifier I will disable (or iterate through versions as needed).
This makes the downstream HTTP PATCH action deterministic.

---

## Containment

### HTTP Disable old secret version (PATCH)
I disable older secret versions using Key Vault REST API calls.

Why I disable instead of delete:
- disabling invalidates old secret material (containment),
- history remains preserved for audit and investigation.

---

## Documentation inside Sentinel

### Incident comment
I add a structured comment describing:
- which secret was involved,
- what containment action I took,
- which version(s) were disabled,
- when the automation ran.

### Update incident
I update incident tags/status so responders can quickly see:
- containment occurred,
- automation executed successfully,
- the incident is ready for review.

📸 `screenshots/08-playbook-design-1.png`  
📸 `screenshots/09-playbook-design-2.png`
