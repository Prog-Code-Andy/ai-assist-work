# Engineering Decision Records

This directory contains the authoritative Engineering Decision Records for AI Assist Work.

## Rules

- Use sequential names such as `EDR-001-phase-gate-governance.md`.
- An accepted EDR is immutable.
- A changed decision requires a new EDR that identifies the record it supersedes or amends.
- Every EDR must record its status, context, decision, consequences, and related records.
- Roadmaps, phases, prompts, reports, and workstation implementation must comply with accepted EDRs.

## Status values

- `Proposed`: written and awaiting user review.
- `Accepted`: explicitly approved by the user and immutable.
- `Superseded`: replaced by a later accepted EDR.
- `Rejected`: reviewed but not adopted.

## Current records

| Record | Decision | Status |
|---|---|---|
| [EDR-001](EDR-001-phase-gate-governance.md) | Phase-gated execution governance | Accepted |
| [EDR-002](EDR-002-local-only-privacy-boundary.md) | Local-only privacy and network boundary | Accepted |
| [EDR-003](EDR-003-system-architecture.md) | Workstation application architecture | Accepted |
| [EDR-004](EDR-004-data-storage-and-retention.md) | Data classification, storage, and retention | Accepted |
| [EDR-005](EDR-005-model-observability-and-evaluation.md) | Model observability, logs, and evaluation | Accepted |
| [EDR-006](EDR-006-learning-process.md) | Required learning process and documentation | Accepted |
| [EDR-007](EDR-007-learning-capture-and-grammar-analysis.md) | Learning capture and grammar-analysis workflow | Accepted |
| [EDR-008](EDR-008-study-materials-reporting-and-export.md) | Study materials, reporting, printing, and export | Accepted |
| [EDR-009](EDR-009-application-wide-diagnostics-and-client-error-capture.md) | Application-wide dual-sink diagnostics and browser error capture | Proposed |

EDR-001 through EDR-008 were explicitly approved on 2026-09-06. They are now immutable. Any material change requires a new EDR that amends or supersedes the affected record.
