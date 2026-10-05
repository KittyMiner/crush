# Escalation Ladder

The escalation ladder defines how operational issues move through responsibility layers without delay or confusion.

## Tier 1: Local Operator Resolution

For issues with low blast radius and clear local ownership.

- local log review
- local configuration check
- re-run targeted validation
- confirm whether the issue is reproducible

## Tier 2: Fleet Specialist Escalation

When the issue crosses a domain boundary or requires specialist diagnosis.

- Calibration if drift or baseline mismatch is suspected
- Coordination if contention, stale state, or scheduling issues are suspected
- Health if service degradation or recovery quality is failing
- Optimization only after validation and baseline confidence are established
- Compliance if policy or traceability risk appears
- Research if root cause remains uncertain

## Tier 3: Fleet Orchestrator Escalation

When the issue crosses system boundaries or impacts multiple critical services.

- define new mission scope
- re-prioritize backlog
- assign specialist team
- coordinate mitigation and communication

## Tier 4: Executive/Policy Escalation

For systemic, policy-sensitive, or high-risk incidents.

- governance review
- wider incident communication
- rollback authority or service isolation directive
- operational freeze if needed

## Escalation Rules

- Escalate when evidence is incomplete or the blast radius is uncertain.
- Escalate when a proposed fix lacks rollback or validation plan.
- Escalate when multiple specialist domains intersect.
- Never delay escalation because the local team wants to “try one more thing.”

