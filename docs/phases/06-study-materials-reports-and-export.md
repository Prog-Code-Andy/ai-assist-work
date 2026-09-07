# Phase 06: Study Materials, Reports, and Export

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/06-study-materials-reports-and-export.md`
- Dependencies: Completed Phase 05 and explicit Phase 06 approval
- Governing EDRs: EDR-001, EDR-002, EDR-003, EDR-004, EDR-005, EDR-006, EDR-007, EDR-008
- Required contract: Report Output Contract v1.0

## Objective

Allow the user to query, study, preview, print, and export trustworthy vocabulary and sentence-level learning history.

## Approved scope

- Build the Study Materials workspace with Vocabulary, Sentences and Grammar, Report Builder, and Downloads views.
- Implement date presets and custom range, input source, writing mode, model, grammar, vocabulary, status, confidence, frequency, search, uniqueness, and sorting filters.
- Use deterministic SQLite queries for all factual rows, dates, and counts.
- Implement the Vocabulary Report exactly as Report Output Contract v1.0, including top-level canonical uniqueness and nested senses.
- Implement the historical Sentences and Grammar Report as one self-contained row per finding with complete Original and Corrected sentences repeated intentionally.
- Ensure no tabs or cross-file lookup are required to understand a generated sentence report row.
- Add weekly, 14-day, monthly, all-time, and custom Learning Summary generation.
- Add optional local-LLM study guidance, progress narrative, and exercises clearly labeled as generated guidance.
- Add preview and local report-history metadata.
- Export self-contained HTML, print-ready HTML for browser Save as PDF, user-facing UTF-8 CSV, and versioned JSON.
- Provide optional normalized technical data export separately from the default sentence-learning CSV.
- Add Application Control settings for reports and export.

## Out of scope

- Automatic scheduled reports.
- Cloud export or sharing.
- Direct PDF renderer unless local maintainability is separately validated within scope and does not replace print-ready HTML.
- Fine-tuning or automatic dataset approval.

## Deliverables

- Study Materials UI and report builder.
- Versioned report-query layer.
- Vocabulary, sentence/grammar, and combined summary renderers.
- HTML, CSV, and JSON exporters.
- Print stylesheet and offline report behavior.
- Report-history metadata and protected local file management.
- Phase report and reporting/export learning notes.

## Acceptance criteria

- Outputs match every required Report Output Contract v1.0 field and behavior.
- Vocabulary default view displays each canonical word once and retains senses and occurrence count.
- A sentence with four findings produces four historical report/CSV rows, each containing complete Original, Corrected, exact change, rule, explanation, examples, and linkage identifier.
- Filtering a CSV to one error category leaves every remaining row understandable by itself.
- SQL-derived facts and optional LLM guidance are visibly distinct.
- Empty filters return an explicit empty result and never invented material.
- HTML opens offline, contains no CDN dependency, escapes user text, and prints clearly.
- CSV opens as UTF-8 and protects against spreadsheet formula injection.
- JSON identifies its contract version and preserves relationships.
- Custom date boundaries respect the configured local time zone.
- Export files are copies and deletion does not silently delete SQLite source records.

## Targeted test plan

- Golden-output contract tests for vocabulary and sentence examples.
- One-sentence/multiple-finding flattening tests.
- Canonical word/sense/occurrence grouping tests.
- Date range, time-zone boundary, combined-filter, sorting, and empty-result tests.
- HTML offline, escaping, print-layout, and no-external-asset tests.
- CSV encoding, columns, formula-injection, and Excel-filter self-containment tests.
- JSON schema and contract-version tests.
- LLM-guidance labeling and factual-boundary tests.
- Existing Writer through Vocabulary smoke tests.

## Manual and privacy validation

- Generate sanitized seven-day vocabulary and sentence reports.
- Filter the sentence CSV in a spreadsheet and confirm every row remains self-contained.
- Disconnect or block external network access and open the HTML report.
- Print or Save as PDF and inspect page breaks.
- Preview exports and verify no diagnostics or unsaved content is included.

## Learning deliverables

- Explain SQL aggregation, normalized storage, denormalized reports, joins, contract tests, date ranges, HTML print styles, CSV encoding/security, JSON versioning, and generated-versus-factual content in English.

## Completion report

Create `reports/phases/06-completion.md` with contract compliance matrix, sample sanitized outputs, tests, print/CSV validation, privacy checks, and limitations.

## Rollback guidance

Document how to disable report generation, remove derived exports safely, retain source learning records, and roll back report metadata migrations.

## Approval gate

Set Phase 06 to complete and stop at `awaiting_approval`. Do not begin Phase 07 without `Approve Phase 07`.
