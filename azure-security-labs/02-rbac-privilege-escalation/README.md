# Lab 02 – RBAC Privilege Escalation Detection

## Objective
Detect and investigate privileged role assignment changes in Azure subscriptions using Microsoft Sentinel.

## Skills Demonstrated
- Azure RBAC monitoring
- Privilege escalation detection
- AzureActivity log analysis
- Sentinel analytic rule creation
- Incident investigation

## Environment
- Microsoft Sentinel (law-central-security)
- AzureActivity logs
- Subscription-level RBAC

---

## Step 1 – RBAC Activity Telemetry
AzureActivity logs are used to capture role assignment changes at subscription scope.

Screenshot:
- AzureActivity query showing ROLEASSIGNMENTS operations

---

## Step 2 – Create RBAC Escalation Analytic Rule
A Sentinel analytic rule is created to detect role assignment creation or deletion at subscription scope.

Screenshot:
- Analytic rule configuration

---

## Step 3 – Trigger Privilege Escalation Event
A privileged RBAC role (Contributor/Owner) is assigned at subscription scope to trigger the detection.

Screenshot:
- Role assignment created in IAM

---

## Step 4 – Incident Generation and Investigation
The RBAC change generates a Sentinel incident containing caller, scope, and correlation details.

Screenshot:
- Sentinel incident details

---

## Outcome
Privileged access changes are detected, investigated, and recorded through Microsoft Sentinel.
