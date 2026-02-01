# degraded mode policy

## purpose

defines system behavior when semantic verification cannot complete.

verification can fail for many reasons:
- service unavailable
- configuration missing or corrupt
- parsing error
- timeout
- missing context required for evaluation

the question is not "will failures happen" but "what does the system do when they happen."

## modes

### fail-closed

verification failure = operation blocked.

when to use:
- high cost of false negative (missing an attack)
- irreversible operations
- regulated environments requiring audit trail

behavior:
- operation does not proceed
- error logged with reason code
- operator notified
- manual override path available (see below)

### fail-open with audit

verification failure = operation proceeds with mandatory logging and alert.

when to use:
- high cost of false positive (blocking legitimate operations)
- reversible operations
- development/testing environments

behavior:
- operation proceeds
- audit log entry required for this mode
- alert generated for review
- time-bounded review target set per deployment

### explicit override

human operator confirms proceed despite verification failure.

requirements:
- operator identity verified
- justification recorded
- expiry set (override is not permanent)
- full audit trail

this is not a "skip" button. it's a documented decision with accountability.

## domain defaults

| domain | default mode | rationale |
|--------|--------------|-----------|
| firmware updates | fail-closed | bricked device risk; supply chain integrity |
| fintech transactions | fail-closed + fast override | regulatory audit requirement; latency pressure |
| ci/cd development | configurable (often fail-open) | development velocity; reversible |
| ci/cd production release | fail-closed | release integrity; irreversible distribution |
| high-value crypto | fail-closed | irreversible loss |
| low-value crypto | configurable | user preference; friction tolerance |

defaults are starting points. operators configure based on their risk tolerance and regulatory requirements.

## configuration

```yaml
degraded_mode:
  default: fail_closed
  
  overrides:
    - context: "ci_dev"
      mode: fail_open_with_audit
      audit_retention_days: 90
      
    - context: "firmware"
      mode: fail_closed
      override_path: manual_approval
      override_expiry_hours: 4
```

## audit requirements

every degraded mode event is logged:

```json
{
  "timestamp": "2026-02-01T12:00:00Z",
  "event_type": "degraded_mode_triggered",
  "mode": "fail_closed",
  "reason": "verification_timeout",
  "context": "firmware_update",
  "operation_id": "op-12345",
  "outcome": "blocked",
  "override": null
}
```

if override used:

```json
{
  "timestamp": "2026-02-01T12:05:00Z",
  "event_type": "degraded_mode_override",
  "operator": "operator-id",
  "justification": "verified manually via separate channel",
  "expiry": "2026-02-01T16:05:00Z",
  "operation_id": "op-12345",
  "outcome": "proceeded_with_override"
}
```

## relationship to other protocols

- appeal and resolution: handles false positives after the fact; degraded mode handles failures during evaluation
- error cost framework: informs which default mode is appropriate for a domain
- invariant lifecycle: invariant bugs may cause evaluation failures; lifecycle process addresses root cause
