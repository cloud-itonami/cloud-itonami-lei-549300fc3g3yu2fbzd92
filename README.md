# cloud-itonami-lei-549300fc3g3yu2fbzd92

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by The Southern Company.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**The Southern Company**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: The Southern Company
- **LEI (ISO 17442)**: [549300FC3G3YU2FBZD92](https://search.gleif.org/#/record/549300FC3G3YU2FBZD92) (GLEIF-verified)
- **Jurisdiction**: US-GA
- **Website**: https://www.southerncompany.com
- **Ticker**: SO (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `facts.edn` — 15 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljk           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO URLs were fetched and fifteen facts recorded — the LEI record
(entity **ACTIVE**, registration **ISSUED**, `FULLY_CORROBORATED` / `CONFORMING`;
entity status and registration status are different fields and are recorded
separately), its 44 ISINs (counted from `meta.pagination.total`, not mirrored),
its managing LOU and LEI-issuer accreditation (Bloomberg Finance L.P.),
registration authority `RA000602` (Division of Corporations, Delaware Department
of State, file `397021`), ISO 20275 legal form `XTIQ` (US-DE Corporation),
reporting exceptions at both consolidation levels (`NON_CONSOLIDATING` — this is
the top of its own group), and **six direct children** each recorded as its own
entity: Alabama Power Company, Georgia Power Company, Mississippi Power Company,
Southern Power Company, Southern Company Gas, and Southern Electric Generating
Company. Nine of the eleven URLs answered `200` when the file was written; the
`direct-parent` and `ultimate-parent` endpoints answered `404` because GLEIF
publishes the exception side of that pair for this entity, which the checker
treats as a fact rather than a failure.

One thing the registry says that this README did not: GLEIF's
`legalJurisdiction` for this entity is **US-DE** (incorporated in Delaware, legal
address c/o Corporation Service Company, Wilmington). The `US-GA` above is the
headquarters address (30 Ivan Allen Jr. Boulevard, Atlanta), which GLEIF records
as a separate field. `facts.edn` carries both; `blueprint.edn` still says `US-GA`
and is left as written here so that the discrepancy is visible rather than
silently corrected.

The checker's exit codes are three, not two: `0` every recorded fact matches the
live sources, `1` a citation broke or a fact drifted, `3` the check could not be
performed at all — an absent `facts.edn`, or every request failing at the
transport level. A check that could not run must not be indistinguishable from a
check that ran and found nothing, so it refuses to report a pass rather than
exiting 0.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
