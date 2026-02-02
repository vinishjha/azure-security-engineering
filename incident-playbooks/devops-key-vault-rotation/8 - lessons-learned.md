# Lessons Learned (what I learned while building this)

This file is intentionally separate from the main documentation so the rest of the repo reads like clean implementation documentation.

## What I learned technically

1) **Identity clarity matters**
I learned to keep client ID, object ID, and secret value clearly separated and labeled, because the whole system depends on correct identity wiring.

2) **Detection depends on real telemetry shape**
I learned to build KQL defensively so it continues to work even when some fields are absent or formatted differently.

3) **Automation needs deterministic parsing**
I learned to use Compose steps to normalize values (vault name, secret name, version) so HTTP actions operate on clean inputs.

4) **Containment must preserve audit history**
I learned that disabling old secret versions is often the right enterprise move because it invalidates old credentials while keeping evidence intact.

5) **Evidence belongs inside the incident**
I learned to write incident comments/tags/status updates so the response becomes self-documenting for audit and post-incident review.

## What I would improve next

- Convert more operations to least-privilege custom roles where possible.
- Add automated notification and ticketing hooks.
- Add correlation detections (RBAC changes + secret reads + pipeline edits) for stronger confidence scoring.
