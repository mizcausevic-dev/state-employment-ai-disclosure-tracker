# Changelog

## [0.1] — 2026-05-29

### Added

- Initial schema with 10-state lifecycle taxonomy + 6-vehicle regulatory_vehicle taxonomy.
- 10-decision covered_decisions taxonomy + 9-actor covered_actors taxonomy + 17-doctrine obligation_kinds.
- Jurisdiction supports `US-XX` AND `US-XX-CITY` patterns — the only Suite tracker that supports sub-state jurisdictions (NYC LL 144 is THE headline employment-AI law).
- Conditional schema requirements (effective_date / sunset_date / supersedes_event_id).
- Node verifier — schema (exit 1) / state-machine (exit 2) / supersedes-reference (exit 3) gates.
- Seed: US-NY-NYC (LL 144 effective), US-IL (820 ILCS 42 + HB 3773 amendment chain), US-MD (HB 1202 facial-recognition), US-CA (AB 331 withdrawn → DFEH rulemaking), US-CO (SB 24-205 effective for employment decisions).
- CI workflow.

### Not yet

- Additional states (NJ S 1588, MA H 1873, WA HB 1951 + more) added per PR.
- Cross-jurisdiction obligation_kinds diff (e.g. what's NYC + IL + CO say vs each other on annual-bias-audit-required).
- Effective-date-as-of query helper.
