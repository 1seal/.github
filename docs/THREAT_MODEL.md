# threat model

## scope

this document defines what 1seal protects against and what it does not protect against.

## what 1seal protects against

### payload substitution

attack: the payload submitted for signing is not what the operator intended.

examples:
- build artifact swapped between build and sign
- transaction parameters modified in memory
- config file replaced before deployment

how 1seal helps: verifies payload against invariants before signing proceeds.

### look-alike attacks

attack: identifiers that visually or semantically resemble legitimate targets.

examples:
- address poisoning (similar wallet addresses)
- typosquatting (similar domain names)
- homoglyph substitution (unicode lookalikes)

how 1seal helps: similarity checks against known-good allowlists.

### policy drift

attack: configuration or policy changed without proper review.

examples:
- security settings weakened
- permissions expanded
- thresholds lowered

how 1seal helps: invariants encode expected policy state; drift detected at verification.

### context injection

attack: manipulation of context that influences signing decisions.

examples:
- agent memory injection
- environment variable manipulation
- metadata tampering

how 1seal helps: invariants verify context integrity alongside payload.

### parameter tampering

attack: modification of parameters between user intent and signed payload.

examples:
- transaction amount changed
- recipient modified
- timestamp backdated

how 1seal helps: invariants check parameter values against expected ranges and allowlists.

## what 1seal does not protect against

### compromised signing key

if the signing key itself is compromised, 1seal cannot help. the attacker can sign anything.

mitigation: hsm/kms, key rotation, access controls.

### root-level system compromise

if the system running 1seal is fully compromised at root level, the attacker can bypass or modify verification.

mitigation: system hardening, integrity monitoring, secure boot.

### malicious operator with full access

if the authorized operator is the adversary, they can configure invariants to allow their attack.

mitigation: access controls, separation of duties, audit logging.

### blockchain consensus attacks

1seal operates before signing. it does not protect against attacks at the consensus layer (51% attacks, finality issues, etc).

mitigation: protocol-level security, finality assumptions.

### smart contract logic bugs

1seal verifies that what you're signing matches intent. it does not verify that the smart contract you're interacting with is safe.

mitigation: smart contract audit, formal verification.

## trust assumptions

### signing infrastructure is not compromised

we assume the signing key and signing process are secure. 1seal is a pre-signing check, not a replacement for signing security.

### invariants are correctly defined

we assume invariants accurately represent intended policy. misconfigured invariants may allow attacks or block legitimate operations.

### operator is not the adversary

we assume the operator configuring 1seal is acting in good faith. malicious operators can configure permissive invariants.

### local environment is not fully compromised

we assume the environment running 1seal has basic integrity. full root compromise can bypass any local verification.

## adversary model

### adversary can

- manipulate payloads before signing
- inject context into agent memory or environment
- generate look-alike identifiers
- exploit gaps in invariant coverage
- attempt to trigger false positives to cause operational disruption

### adversary cannot

- compromise the signing key (that's out of scope)
- modify 1seal verification logic (if integrity is maintained)
- bypass mandatory audit logging (if logging infrastructure is intact)
- override without trace (if audit is functioning)

## design implications

given this threat model:

1. 1seal focuses on semantic verification, not key protection
2. invariants must be carefully designed to match actual threats
3. audit logging is mandatory and immutable where possible
4. degraded mode and appeal protocols exist because false positives are expected
5. 1seal complements existing security layers, not replaces them
