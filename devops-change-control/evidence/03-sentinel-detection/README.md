# Evidence 03 — Sentinel Detection: Policy Denied Deployment

## Signal
AzureActivity shows a failed deployment with `RequestDisallowedByPolicy` in prod.

## Detection
A scheduled analytics rule creates an incident when policy denies automated deployments.

## Evidence
- kql-results-policy-deny.png
- sentinel-rule-config.png
- sentinel-incident-created.png

