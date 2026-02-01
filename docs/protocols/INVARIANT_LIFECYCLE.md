# invariant lifecycle protocol

## purpose

defines how rules are created, promoted, deprecated, and retired.

static rules become false-positive generators. what was valid yesterday may block legitimate operations tomorrow. invariants need a lifecycle.

## stages

### proposal

new invariant proposed.

triggers:
- failure mode identified
- threat model update
- incident response learning

requirements:
- evidence of failure mode or risk
- proposed rule definition
- expected fp/fn characteristics
- scope (which domains/contexts)

next stage: experimental

### experimental

warn-only mode to gather data.

duration: 7-30 days depending on traffic volume

behavior:
- evaluation runs
- results logged
- no blocking (warn only)

metrics collected:
- false positive rate
- false negative rate (if measurable)
- performance impact
- edge cases discovered

exit criteria:
- fp rate below threshold
- no critical issues discovered
- sufficient sample size

next stage: active (if criteria met) or rejected (if not)

### active

enforcing mode.

duration: until deprecated

behavior:
- evaluation runs
- blocking enabled
- appeals processed per appeal protocol

monitoring:
- ongoing fp tracking
- appeal rate
- override rate
- performance

review cadence: quarterly

triggers for review:
- fp rate exceeds threshold
- high appeal rate
- threat landscape change
- incident post-mortem

next stage: deprecated (if obsolete or problematic)

### deprecated

sunset period.

duration: 30 days

behavior:
- warn-only (no blocking)
- operators notified of pending removal
- migration guidance provided if replacement exists

purpose:
- allow operators to adjust
- catch any dependencies
- gather final feedback

next stage: retired

### retired

removed from enforcement.

behavior:
- no evaluation
- rule definition archived for audit
- historical data preserved

restoration:
- can be re-proposed if threat re-emerges
- follows standard proposal process

## governance

who proposes: research team, operators, incident response

who approves promotion to active: defined per deployment (typically requires quorum for production systems)

who initiates deprecation: research team with operator notification

who can restore: same as proposal

## versioning

rule sets are versioned.

format: semantic versioning

major: breaking changes to rule behavior
minor: new rules added
patch: rule definition fixes

changelog maintained for audit.

## configuration

```yaml
invariant_lifecycle:
  experimental_duration_days: 14
  deprecated_sunset_days: 30
  review_cadence_days: 90
  
  promotion_criteria:
    fp_rate_threshold: 0.01
    min_sample_size: 1000
    
  deprecation_triggers:
    fp_rate_threshold: 0.05
    appeal_rate_threshold: 0.10
```

## relationship to other protocols

- appeal and resolution: high appeal rate may trigger invariant review
- error cost framework: informs fp/fn thresholds for promotion criteria
- degraded mode: invariant bugs may cause evaluation failures
