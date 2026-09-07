# Phase Prompt 06: Study Materials, Reports, and Export

Execute this prompt only through `prompts/MASTER.md` after Phase 05 is complete and `Approve Phase 06` is validated.

## Required specifications

Read and implement only:

- `docs/phases/06-study-materials-reports-and-export.md`;
- `docs/contracts/report-output-contract-v1.md`.

## Execution instructions

1. Treat Report Output Contract v1.0 as an exact acceptance contract.
2. Build Study Materials, filters, previews, report history, and local export from deterministic SQLite facts.
3. Keep optional LLM study guidance clearly labeled and prevent it from inventing rows, dates, counts, or errors.
4. Display canonical vocabulary once with nested senses and occurrence counts.
5. For historical sentence reports and the default CSV, emit one self-contained row per finding and repeat complete Original and Corrected sentences intentionally.
6. Verify that Excel filtering never separates an error from its context, rule, explanation, and examples.
7. Generate offline self-contained HTML, print-ready output, safe UTF-8 CSV, and versioned JSON without external assets.
8. Do not add automatic schedules or fine-tuning.
9. Produce a contract compliance matrix, report, English learning documentation, update state, request `Approve Phase 07`, and stop.
