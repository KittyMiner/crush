# Diagnostics Protocol

The diagnostics protocol is the first mandatory step in any fleet mission. It converts uncertainty into measurable signal and prevents wasted motion.

## Purpose

To establish a defensible understanding of the current operating state before action is taken.

## Required Sequence

1. Define the incident or mission scope.
2. Confirm the target surface: service, worker, node, queue, dependency, or agent.
3. Capture baseline conditions and recent changes.
4. List symptoms, signals, and their time correlation.
5. Gather operational evidence: logs, metrics, traces, alerts, config diffs, deployment history.
6. Separate facts from hypotheses.
7. Identify likely failure domains.
8. Rank hypotheses by confidence and impact.
9. Request missing evidence if the signal is incomplete.
10. Decide whether to continue to remediation or to gather more evidence.

## Evidence Categories

- Runtime signals: latency, throughput, error rate, queue depth, saturation, retries
- Dependency signals: downstream failures, timeout rates, connection instability, availability
- Coordination signals: lock waits, contention, stale state, duplicate work, leader instability
- Health signals: restart loops, crash loops, degraded service windows, recovery time
- Policy signals: privilege drift, secret handling, identity mismatches, missing traceability

## Non-Negotiable Rules

- No issue is “understood” without direct evidence.
- No root cause is accepted without a chain of reasoning and supporting signal.
- A missing metric is a gap, not a conclusion.
- A weak signal is a hypothesis, not a fact.

## Diagnostic Output

Each diagnostic pass must produce:

- problem statement
- observed evidence
- likely failure domains
- hypothesis ranking
- missing evidence list
- recommended next action

## Kimmi K4 Standard

Diagnostics must be precise enough for engineering follow-through and honest enough to admit uncertainty.

