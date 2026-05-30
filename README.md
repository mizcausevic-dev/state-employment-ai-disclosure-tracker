# state-employment-ai-disclosure-tracker

> **State Employment AI Disclosure Tracker v0.1 draft.** A per-jurisdiction lifecycle ledger of US state legislative statutes, local ordinances, state civil-rights-agency guidance, and state DOL pre-rule guidance governing employment-AI tools — from `proposed` to `in-committee` / `passed-one-chamber` to `enacted-awaiting-effective` to `effective` to `amended` / `superseded` / `sunset` / `vetoed` / `withdrawn`. The Operator surface an employer, AI vendor, or bias auditor queries to know **which jurisdiction's employment-AI obligations govern a hiring / promotion / performance / termination decision on a given date**.

Part of the [Kinetic Gain Protocol Suite](https://suite.kineticgain.com).

> Status: v0.1 draft. Schema at [`schema/disclosure-event.schema.json`](./schema/disclosure-event.schema.json), per-jurisdiction streams at [`states/`](./states/), Node verifier at [`src/verify.mjs`](./src/verify.mjs).

## Why this exists

Employment-AI regulation is fragmenting at three levels: **federal floor** (EEOC AI Guidance May 2023, Title VII, ADA, ADEA, GINA, OFCCP), **state statutes** (IL AI Video Interview Act, MD HB 1202, CO SB 24-205, CA AB 331-or-DFEH-regs, and 12+ proposed bills), and **local ordinances** (NYC Local Law 144 is the headline). An employer hiring a NYC-resident candidate via a video-interview AI tool operated by a federal contractor headquartered in CO is bound by NYC LL 144 AEDT + EEOC AI Guidance + OFCCP regs + CO SB 24-205 + IL 820 ILCS 42 (if the role is partly IL-based) + Title VII. Decision Cards have to reference the obligation set in force at the decision date. This lifecycle ledger answers "what is in force for this jurisdiction on this date" deterministically.

## The shape

| Field | Purpose |
| --- | --- |
| `event_id`, `jurisdiction`, `timestamp` | Append-only stamped identity. `jurisdiction` accepts `US-XX` AND `US-XX-CITY` (e.g. `US-NY-NYC`) — the only Suite tracker that supports sub-state jurisdictions, because NYC LL 144 is THE headline employment-AI law |
| `lifecycle_state` | One of 10 (proposed → in-committee → passed-one-chamber → enacted-awaiting-effective → effective → amended → superseded → sunset → vetoed → withdrawn) |
| `regulatory_vehicle` | One of 6 (state-legislative-statute, local-ordinance, state-civil-rights-agency-guidance, state-dol-pre-rule-guidance, state-doi-bulletin, city-or-county-rule) |
| `citation` | Short label + long title + jurisdictional URI + session year |
| `effective_date` / `sunset_date` | REQUIRED when respective lifecycle_state |
| `scope` | 10-decision covered_decisions + 9-actor covered_actors + minimum employer size + geographic scope |
| `obligation_kinds` | 17-doctrine obligation taxonomy including HR-Tech-specific obligations |
| `regulator` | Primary agency + concurrent jurisdiction |
| `supersedes_event_id` | REQUIRED when amended / superseded |

## Seed coverage (v0.1)

| Jurisdiction | Last lifecycle_state | Key citation |
| --- | --- | --- |
| **US-NY-NYC** | `effective` (2023-07-05) | NYC Local Law 144 — first US AEDT-specific bias-audit law |
| **US-IL** | `amended` (effective 2026-01-01) | IL 820 ILCS 42 (Video Interview Act) layered with HB 3773 (PA 104-0049 consequential-decisions) |
| **US-MD** | `effective` (2020-10-01) | MD HB 1202 (Chapter 595) facial-recognition prohibition |
| **US-CA** | `withdrawn` | CA AB 331 held in committee; DFEH/CRD took up FEHA rulemaking instead |
| **US-CO** | `effective` (2026-02-01) | CO SB 24-205 — applies to consequential decisions including employment |

## Quick start

```bash
npm install
npm run verify
```

Exit 0 on success, 1 on schema, 2 on illegal transition, 3 on broken supersedes, 4 on IO.

## Composes with

| Repo | Role |
| --- | --- |
| [`employment-decision-record-audit-stream`](https://github.com/mizcausevic-dev/employment-decision-record-audit-stream) | Per-decision audit events that must conform to whichever obligation_kinds were effective at the time |
| [`state-real-estate-ai-disclosure-tracker`](https://github.com/mizcausevic-dev/state-real-estate-ai-disclosure-tracker) | Sibling PropTech |
| [`state-insurance-ai-disclosure-tracker`](https://github.com/mizcausevic-dev/state-insurance-ai-disclosure-tracker) | Sibling InsurTech |
| [`state-ai-disclosure-state-tracker`](https://github.com/mizcausevic-dev/state-ai-disclosure-state-tracker) | Sibling EdTech |
| [`fda-samd-classification-board`](https://github.com/mizcausevic-dev/fda-samd-classification-board) | Sibling HealthTech regulatory-lifecycle Operator |

## Compliance posture

HR-Tech-readiness scaffolding for state and local employment-AI regulatory tracking. Does not establish compliance with any state law or local ordinance — local employment counsel + state civil-rights / DOL agency engagement remain authoritative. Per the standing public-language guardrail: *readiness · evidence · posture · controls · scaffolding* — never "NYC-LL-144-compliant" or "EEOC-attested" without an external attestation specific to the jurisdiction.

## License

MIT — see [`LICENSE`](./LICENSE).
