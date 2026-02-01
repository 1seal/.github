# error cost framework

## purpose

defines how thresholds are calibrated based on domain-specific error costs.

optimal thresholds depend on context. one-size-fits-all doesn't work. blocking a firmware update has different consequences than blocking a low-value transaction.

## framework

### inputs

`c_fp`: cost of false positive

what happens when we block a legitimate operation?
- delayed update
- blocked transaction
- manual intervention required
- user friction
- regulatory scrutiny

`c_fn`: cost of false negative

what happens when we allow an attack?
- compromised system
- stolen funds
- supply chain breach
- irreversible damage
- regulatory violation

`base_rate`: expected attack frequency

how often do attacks actually occur in this context?
- high base rate: more aggressive detection justified
- low base rate: high fp cost may outweigh detection benefit

### output

threshold and bias direction.

conservative: lower threshold, block more, accept higher fp rate
permissive: higher threshold, block less, accept higher fn rate

## domain analysis

### firmware updates

c_fp: medium
- delayed update
- support burden
- user friction

c_fn: extreme
- bricked device
- supply chain compromise
- mass exploitation potential

base_rate: low but catastrophic when hit

recommendation: conservative threshold

rationale: irreversible damage from fn vastly outweighs temporary fp inconvenience

### high-value financial transactions

c_fp: high
- blocked business
- regulatory scrutiny
- missed response targets
- reputation damage

c_fn: extreme
- irreversible loss
- regulatory violation
- fraud liability

base_rate: varies by context

recommendation: conservative with fast appeal path

rationale: both costs high; need blocking but also rapid resolution

### low-value transactions

c_fp: medium
- user friction
- support burden
- conversion loss

c_fn: medium
- small loss per incident
- aggregate risk if systemic

base_rate: higher than high-value

recommendation: configurable per user preference

rationale: user may prefer convenience over protection for small amounts

### ci/cd development

c_fp: low
- rebuild required
- developer time
- minor delay

c_fn: medium
- delayed detection
- tech debt
- potential propagation

base_rate: lower in dev than prod

recommendation: permissive threshold

rationale: development velocity matters; prod gates catch issues later

### ci/cd production release

c_fp: medium
- delayed release
- coordination overhead
- customer impact

c_fn: extreme
- compromised release
- supply chain attack
- mass distribution of malware

base_rate: low but catastrophic

recommendation: conservative threshold

rationale: production release is high-leverage attack surface

## calibration process

1. identify domain and context
2. estimate c_fp for that context
3. estimate c_fn for that context
4. estimate base_rate from threat intelligence
5. select initial threshold based on cost ratio
6. validate with experimental phase (warn-only)
7. adjust based on observed fp/fn rates
8. document rationale for audit

## recalibration

triggers:
- threat landscape change
- fp rate exceeds threshold
- incident post-mortem reveals gap
- business context change

frequency: quarterly review minimum

process:
- review metrics from monitoring
- compare to calibration assumptions
- adjust if assumptions no longer hold
- document change rationale

## configuration

```yaml
error_cost:
  domains:
    firmware:
      c_fp: medium
      c_fn: extreme
      bias: conservative
      threshold: 0.3
      
    fintech_high_value:
      c_fp: high
      c_fn: extreme
      bias: conservative
      threshold: 0.4
      appeal_response_target_hours: 4
      
    ci_dev:
      c_fp: low
      c_fn: medium
      bias: permissive
      threshold: 0.7
      
    ci_prod:
      c_fp: medium
      c_fn: extreme
      bias: conservative
      threshold: 0.3
```

## relationship to other protocols

- degraded mode: error cost informs default fail-open vs fail-closed
- appeal and resolution: high c_fp domains need faster appeal paths
- invariant lifecycle: promotion criteria use error cost for fp threshold
