# Azure DevOps Approval Gate (Manual Validation)

## Why this exists
Production deployments in enterprise environments are not “push button.”
A human approval step (change review) is commonly required before changes proceed.

## What we implemented
- A pipeline with stages:
  - WhatIf (advisory preview)
  - Approval (manual validation)
  - Deploy (attempts the change, expected to be denied by policy)

## Evidence
See `devops-change-control/evidence/02-ado-approval/`:
- approval UI screenshot
- pipeline stage flow showing gating before deploy
