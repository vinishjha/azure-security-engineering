# DevOps Change Control for Regulated Azure Workloads (NDA-safe)

## Goal
Demonstrate an enterprise-style deployment control pattern where:
- infrastructure changes are defined as code (Bicep)
- deployments run through Azure DevOps pipelines
- production changes are gated by explicit human approval
- guardrails (Azure Policy) deny unsafe configurations even if a pipeline is compromised

This project is intentionally architecture-first:
- “who can deploy” is enforced through identity + scope
- “what can be deployed” is enforced through policy guardrails
- evidence is captured at each control point
