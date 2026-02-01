# security policy

## coordinated vulnerability disclosure

all security research follows coordinated vulnerability disclosure (cvd).

## reporting a vulnerability

if you find a security issue in 1seal:

1. do not open a public GitHub issue
2. email: security@1seal.org
3. include:
   - description of the vulnerability
   - steps to reproduce
   - potential impact
   - suggested fixes (optional)

## what to expect

- acknowledgment target: 48 hours (not a guarantee)
- initial assessment target: 7 days (not a guarantee)
- regular updates while remediation is in progress
- credit in advisory (unless you prefer anonymity)

## scope

this policy covers:
- 1seal specifications and protocols
- documentation that could lead to security issues

out of scope:
- social engineering attacks
- denial of service attacks that don't exploit a bug
- issues in dependencies (report to upstream)

## disclosure timeline

typical remediation targets (not guarantees; severity and complexity dependent):
- critical: ~14 days
- high: ~30 days
- medium/low: ~90 days

we coordinate with reporters on disclosure timing. we do not disclose before fixes are available unless there's active exploitation.

## our research

we conduct security research on trust infrastructure and follow the same cvd principles:

- report through appropriate channels
- provide reasonable time for fixes
- do not disclose details until fixes are released
- coordinate on disclosure timing with maintainers

public statements about our research reference only published advisories where fixes are released and credit is visible.
