# Deployment Gate Checklist

This checklist gives the fleet a safe and defensible rollout standard.

## Gate 1: Mission Readiness

- Problem scope is defined.
- Evidence has been gathered.
- Baseline is established or explicitly marked as missing.
- Success criteria are measurable.

## Gate 2: Safety Review

- Blast radius is understood.
- Rollback plan exists.
- Hidden dependencies are identified.
- Resource and coordination impact is reviewed.

## Gate 3: Policy & Traceability

- Change is attributable and logged.
- Secrets and identities are handled correctly.
- Policy exceptions are explicit and approved.
- Deployment steps are auditable.

## Gate 4: Validation Readiness

- Baseline metrics are available.
- Before/after checks are defined.
- Recovery and rollback verification is planned.
- Health thresholds are known.

## Gate 5: Release Decision

Approval only when:

- evidence supports the change,
- policy is satisfied,
- risk is understood,
- rollback is prepared,
- and validation method is explicit.

## Gate Failure

If any gate fails, the change remains in review. No rollout proceeds on optimism or urgency alone.

