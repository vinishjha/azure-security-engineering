# Validation (How I tested end-to-end)

This document explains how I validated the build works.

---

## Test 1 — I verified the pipeline can retrieve the Key Vault secret
1. I ran the Azure DevOps pipeline.
2. I confirmed the pipeline successfully retrieved the secret.
3. I used this to confirm legitimate access generates real audit telemetry.

📸 `screenshots/03-cicd-secret-received.png`

---

## Test 2 — I verified detection and incident creation
1. I ran the KQL to confirm Key Vault `SecretGet` events appear for the secret.
2. I enabled the analytics rule and waited for the rule schedule.
3. I confirmed an incident is created for matching events.

📸 `screenshots/06-incident-creation.png`  
📸 `screenshots/07-incident-detected.png`

---

## Test 3 — I verified the playbook containment actions
1. I triggered the playbook from the incident.
2. I confirmed the playbook successfully:
   - enriched the incident context,
   - listed secret versions,
   - disabled old secret versions,
   - wrote evidence into the incident.

Expected outcomes:
- old secret version shows as disabled in Key Vault,
- incident contains a comment describing actions,
- incident tags/status reflect that automation ran.

📸 `screenshots/08-playbook-design-1.png`  
📸 `screenshots/09-playbook-design-2.png`
