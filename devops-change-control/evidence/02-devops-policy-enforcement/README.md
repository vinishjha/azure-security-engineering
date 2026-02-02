# Evidence 02 — DevOps Change Path Enforced by Azure Policy

## Scenario
An Azure DevOps pipeline (non-human identity via service connection) attempted to deploy a forbidden storage configuration in protected scope `rg-prod-test`.

## Design
- WhatIf stage is advisory (pipeline continues even if policy denies)
- Deploy stage is enforced (policy deny blocks the change)

## Result
Deploy failed with `RequestDisallowedByPolicy`, referencing the policy assignment scoped to `rg-prod-test`.

## Screenshots
- ado-run-summary.png
- ado-whatif-advisory.png
- ado-deploy-denied-by-policy.png
- policy-assignment-scope.png
