# Agent-to-Agent Handoff Template

This template enables clean, consistent transitions between specialists and the orchestrator.

## Handoff Header

- Mission ID
- Time of handoff
- Current owner
- Target owner
- Mission status
- Priority
- Confidence level

## Handoff Body

### 1. Scope
What is being investigated, and what is not in scope?

### 2. Current State
What is observed right now?

### 3. Evidence
What evidence supports the current understanding?

### 4. Hypotheses
What is believed to be happening, with confidence levels?

### 5. Actions Taken
What has already been tried, and with what outcomes?

### 6. Recommended Next Move
What should the receiving agent do next?

### 7. Risk and Unknowns
What remains uncertain or risky?

### 8. Validation Requirement
What evidence must be captured before closure?

## Example

Mission ID: FLEET-2041
Owner: Fleet Health Specialist
Target: Fleet Coordination Specialist
Priority: High
Confidence: Medium

Current State: queue depth has increased 3x over 18 minutes, but service latency remains within threshold.
Evidence: logs show lock contention at job distributor after restart wave.
Hypotheses: coordinator under contention; recovered state lag causing duplicate claims.
Recommended next move: validate lock fairness and duplicate job claims under restart conditions.
Risk: partial outage may continue while suspicion remains unconfirmed.
Validation: reproduce with restart simulation and compare lock wait metrics.

