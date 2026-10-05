# Remediation Playbook

The remediation playbook governs how the fleet responds once diagnosis is sufficient to act.

## Mission

Reduce the risk and restore control with the fewest, safest, and most reversible interventions possible.

## Remediation Principles

- Prefer targeted intervention over broad churn.
- Prefer reversible changes over irreversible rewrites.
- Prefer evidence-backed fixes over “good-looking” fixes.
- Prefer stabilizers and guardrails when the root cause is uncertain.
- Prefer validation under realistic failure states.

## Playbook Sequence

1. Confirm the problem and the desired end state.
2. Establish the smallest safe intervention set.
3. Consider coordination, health, and policy implications before rollout.
4. Apply one intervention at a time when possible.
5. Re-measure the specific affected signal.
6. Compare against the original baseline.
7. Determine if the system is healthier, stable, or still uncertain.
8. If necessary, iterate or roll back.

## Categories of Remediation

- Calibration correction: baseline and threshold adjustments
- Coordination repair: lock strategy, scheduling, state reconciliation
- Health guardrails: retries, circuit breaking, degraded-mode handling
- Optimization tuning: throughput and efficiency improvements after calibration
- Compliance hardening: permissions, traceability, secret control, deployment policy

## Required Validation Check

Every remediation must include:

- before-state evidence
- after-state evidence
- expected effect
- observed effect
- rollback path
- residual risk

## Stop Conditions

Stop remediation when:

- the issue is resolved and validated,
- the fix is not yet justified by evidence,
- or the intervention increases risk beyond acceptable bounds.

