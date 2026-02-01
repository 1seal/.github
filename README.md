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

## published advisories

security research in software supply chain trust infrastructure:

| CVE            | component                    | patched                      | advisory                                                                                         |
| -------------- | ---------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------ |
| CVE-2026-22703 | sigstore/cosign              | cosign v2.6.2; cosign v3.0.4 | [GHSA-whqx-f9j3-ch6m](https://github.com/advisories/GHSA-whqx-f9j3-ch6m)                         |
| CVE-2026-23831 | sigstore/rekor               | rekor 1.5.0                  | [GHSA-273p-m2cw-6833](https://github.com/sigstore/rekor/security/advisories/GHSA-273p-m2cw-6833) |
| CVE-2026-24117 | sigstore/rekor               | rekor 1.5.0                  | [GHSA-4c4x-jm2x-pf9j](https://github.com/advisories/GHSA-4c4x-jm2x-pf9j)                         |
| CVE-2026-24137 | sigstore/sigstore            | sigstore 1.10.4              | [GHSA-fcv2-xgw5-pqxf](https://github.com/advisories/GHSA-fcv2-xgw5-pqxf)                         |
| CVE-2026-23991 | theupdateframework/go-tuf/v2 | go-tuf/v2 2.3.1              | [GHSA-846p-jg2w-w324](https://github.com/advisories/GHSA-846p-jg2w-w324)                         |
| CVE-2026-23992 | theupdateframework/go-tuf/v2 | go-tuf/v2 2.3.1              | [GHSA-fphv-w9fq-2525](https://github.com/advisories/GHSA-fphv-w9fq-2525)                         |
| CVE-2026-24686 | theupdateframework/go-tuf/v2 | go-tuf/v2 2.4.1              | [GHSA-jqc5-w2xx-5vq4](https://github.com/advisories/GHSA-jqc5-w2xx-5vq4)                         |
| CVE-2026-24845 | chainguard-dev/malcontent    | malcontent 1.20.3            | [GHSA-9m43-p3cx-w8j5](https://github.com/advisories/GHSA-9m43-p3cx-w8j5)                         |
| CVE-2026-24846 | chainguard-dev/malcontent    | malcontent 1.20.3            | [GHSA-923j-vrcg-hxwh](https://github.com/advisories/GHSA-923j-vrcg-hxwh)                         |

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
