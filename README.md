<!-- path: profile/README.md -->

# 1seal

Offline pre-transaction security for AI agents and server-side wallet APIs.

We build deterministic guardrails that sit **after policy/simulation and immediately before the signing boundary** (MPC/HSM). The goal is to prevent “what you see ≠ what gets signed” failures—address poisoning, alias confusables, and last-mile drift—by validating the **canonical payload** right before execution.

## What we build

**SealGuard** (beta) — a stateless pre-sign validator that canonicalizes the transaction payload and returns an auditable verdict (`ok / step-up / block`) with deterministic reason codes.

**TxSeal** — emits DSSE receipts for signing decisions and evidence so flows can be verified offline. Includes a zero-runtime-deps DSSE+JCS verifier.

**PoisonAtlas** — dataset format for address-poisoning research and evaluation.

## Status

Private beta. The core engine and full rule sets are iterated with a small set of design partners. Expect breaking changes until a 1.x GA release is announced.

Security reports: please follow `SECURITY.md` in this org’s `.github` repository.
