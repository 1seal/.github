# 1seal

last-mile verification for trust infrastructure.

## what is this

1seal is designed to provide semantic verification immediately before signing or authorization. it's the layer that checks whether the payload matches intent — complementing identity (pki/sigstore), provenance (slsa/in-toto), and integrity (checksums/transparency logs).

## status

this repository is documentation and specification first. implementation remains private. no feature promises or timelines.

what's here:
- thesis and positioning
- operational protocol specifications
- threat model and non-goals

what's not here:
- implementation code
- detection algorithms

## the problem

<<<<<<< Updated upstream
every signing system has the same gap:

- pki/sigstore answers "who signed this"
- slsa/in-toto answers "where was this built"
- checksums/transparency logs answer "what bytes moved"

none of these verify whether the payload matches intent.

that gap is where manipulation attacks live: payload substitution, address poisoning, config drift, context injection. the signature is valid. the provenance is clean. and the thing you signed is not what you meant to sign.

## where 1seal fits

	layer 5: authorization (opa, cedar)
	layer 4: semantics (1seal) ← here
	layer 3: provenance (slsa, in-toto)
	layer 2: integrity (checksums, rekor)
	layer 1: identity (pki, sigstore/fulcio)

## operational protocols

security controls must include brakes. see [docs/protocols/](docs/protocols/) for:

- degraded mode policy — what happens when verification cannot complete
- appeal and resolution — how false positives are handled
- invariant lifecycle — how rules are created, promoted, deprecated, retired
- error cost framework — how thresholds are calibrated per domain

all research follows coordinated vulnerability disclosure (cvd).

## threat model

see [docs/THREAT\_MODEL.md](docs/THREAT_MODEL.md) for:
- what 1seal protects against
- what 1seal does not protect against
- trust assumptions
- adversary model

## security policy

see [SECURITY.md](SECURITY.md) for vulnerability reporting.

## contact

interested in design partner conversations: hello@1seal.org

---

## docs structure

	docs/
	├── protocols/
	│   ├── DEGRADED_MODE.md
	│   ├── APPEAL_RESOLUTION.md
	│   ├── INVARIANT_LIFECYCLE.md
	│   └── ERROR_COST_FRAMEWORK.md
	├── THREAT_MODEL.md
	└── NON_GOALS.md
=======
Security reports: please follow `SECURITY.md` in this org’s `.github` repository.

## specs and protocols

this org’s public spec artifacts live in `docs/`:

- `docs/THREAT_MODEL.md`
- `docs/NON_GOALS.md`
- `docs/protocols/DEGRADED_MODE.md`
- `docs/protocols/APPEAL_RESOLUTION.md`
- `docs/protocols/INVARIANT_LIFECYCLE.md`
- `docs/protocols/ERROR_COST_FRAMEWORK.md`
