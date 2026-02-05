## Conditional Access

- Tier-0 policy: `CA-T0-Admins-Strong-MFA`
- Baseline policy: `CA-Base-Block-Legacy-Authentication`

Tiering is encoded in naming to support reliable scoping in detections.

## PIM

- Privileged role assigned as **Eligible** (not permanent)
- Activation requires MFA + justification (+ approval if configured)
- Goal: eliminate standing privileged access.
