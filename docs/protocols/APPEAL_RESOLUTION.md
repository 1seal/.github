# appeal and resolution protocol

## purpose

defines how false positives are handled.

false positives are inevitable in any security system. the question is whether there's a path to resolve them without disabling the control entirely.

## flow

### 1. block

operation blocked with:
- reason code
- evidence snapshot
- timestamp
- operator notification

### 2. request

operator requests review with:
- block reference
- counter-evidence
- justification

acknowledgment target (recommended default): 24h (deployment-defined).

### 3. review

review against invariant definition:
- is this a rule bug?
- is this an actual violation?
- is there missing context?

### 4. outcome

one of:
- override granted (false positive confirmed)
- invariant updated (rule was wrong)
- block upheld (true positive confirmed)
- escalation (complex case)

### 5. resolution

action taken and recorded:
- who reviewed
- decision rationale
- evidence considered
- timestamp

## time bounds

these are deployment-defined targets, not guarantees. recommended defaults:

initial response target: 24h

resolution target: 72h for standard cases, 1 week for complex

escalation path: defined per deployment

## audit requirements

every appeal is logged:

```json
{
  "timestamp": "2026-02-01T12:00:00Z",
  "event_type": "appeal_request",
  "block_reference": "blk-12345",
  "operator": "operator-id",
  "counter_evidence": "description of why this is legitimate",
  "status": "pending_review"
}
```

every resolution is logged:

```json
{
  "timestamp": "2026-02-01T14:00:00Z",
  "event_type": "appeal_resolution",
  "block_reference": "blk-12345",
  "reviewer": "reviewer-id",
  "outcome": "override_granted",
  "rationale": "confirmed false positive: legitimate configuration change",
  "evidence_reviewed": ["counter-evidence-hash", "audit-log-hash"]
}
```

## override mechanics

if override granted:
- operation may proceed
- override has expiry (not permanent)
- full audit trail preserved
- invariant review may be triggered

## relationship to other protocols

- degraded mode: handles failures during evaluation; appeal handles disputes after evaluation
- invariant lifecycle: if appeal reveals rule bug, feeds into invariant update process
- error cost framework: high appeal rate may indicate threshold miscalibration
