# non-goals

## purpose

explicit documentation of what 1seal does not try to do.

clarity about non-goals prevents scope creep and sets accurate expectations.

## identity verification

1seal does not verify identity.

that's the job of pki, sigstore/fulcio, spiffe, and similar systems.

we assume identity is already verified by the time 1seal runs.

## provenance verification

1seal does not verify provenance.

that's the job of slsa, in-toto, guac, and similar systems.

we assume provenance is tracked separately.

## integrity verification

1seal does not verify integrity.

that's the job of checksums, merkle trees, rekor, and similar systems.

we assume integrity is verified at appropriate points.

## authorization

1seal does not make authorization decisions.

that's the job of opa, cedar, rego, and similar policy engines.

1seal runs before authorization to verify semantic integrity.

## key management

1seal does not manage signing keys.

that's the job of hsm, kms, vault, and similar systems.

we assume keys are managed securely.

## threat intelligence

1seal does not provide threat intelligence.

we provide invariant definitions based on known failure modes.

threat intelligence feeds may inform invariant updates, but that's a separate concern.

## incident response

1seal does not perform incident response.

we provide blocking and logging. investigation and remediation are separate processes.

## compliance certification

1seal does not certify compliance.

we provide evidence (audit logs, verdicts) that may support compliance programs.

compliance determination is a separate process.

## replacing human judgment

1seal does not replace human judgment.

we provide automated checks. humans configure invariants, review appeals, and make final decisions.

the appeal and resolution protocol explicitly includes human review.

## why document non-goals

1. prevents feature creep into areas where better solutions exist
2. sets accurate expectations for adopters
3. clarifies integration points with other systems
4. guides development prioritization
5. supports the principle of doing one thing well
