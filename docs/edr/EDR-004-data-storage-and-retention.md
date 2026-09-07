# EDR-004: Data Classification, Storage, and Retention

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

The product must support writing history, English-learning analysis, vocabulary, reports, model metrics, and troubleshooting without silently mixing sensitive content with operational logs or future training data.

## Decision

Data is separated into four classes:

| Class | Purpose | Default content policy |
|---|---|---|
| Diagnostic logs | Errors and application events | No message bodies |
| Performance metrics | Tokens, timing, resources, model settings | No message bodies |
| Learning data | Original text, rewrites, grammar, vocabulary | Explicit opt-in |
| Evaluation data | Model result, user final edit, rating and reason | Explicit opt-in |

Storage rules:

1. SQLite is the authoritative structured store for settings, sessions, entries, generations, grammar findings, vocabulary, metrics, feedback, and report metadata.
2. Structured JSONL files hold rotating diagnostic events suitable for local troubleshooting.
3. Diagnostic logs, learning data, and evaluation data must remain logically separable and independently deletable.
4. Retention is configurable by data class, with supported presets and a custom value.
5. JSONL logs rotate by size and age and obey a total disk-usage limit.
6. Application Control shows current storage use, retention, export, and deletion controls.
7. Raw audio is not retained by default.
8. Generated HTML reports and diagnostic bundles are user-created exports, not hidden application uploads.
9. SQLite schema and exports must preserve model name/version, prompt/configuration version, writing mode, timestamps, and feedback needed for reproducible evaluation.
10. User content is never considered fine-tuning data merely because it exists in logs or SQLite. Training-dataset inclusion requires a separate explicit selection and privacy review.
11. Voice recordings and voice transcripts are different data classes: temporary raw audio may be deleted while an explicitly eligible transcript is retained as learning data.
12. A stored learning entry must preserve its input source (`typed` or `voice`) and link the original input, model rewrite, user-edited final text, grammar findings, examples, and user feedback without copying message bodies into diagnostic logs.
13. Vocabulary uses canonical records rather than duplicate word rows. One normalized lemma and language identify a vocabulary item; distinct parts of speech and meanings are stored as senses; every dated use is stored as an occurrence linked to the canonical item and relevant sense.
14. Report files are derived exports. Deleting an exported file does not delete its source learning records, and deleting source records requires the protected data-management workflow.

The exact retention defaults and database schema will be decided in approved roadmap and phase specifications.

## Consequences

- Operational troubleshooting can occur without exposing stored message content.
- Users can retain learning history while deleting diagnostics, or the reverse.
- Future evaluation and fine-tuning remain possible without treating low-quality logs as training examples.
- Schema migrations and data-management UI are required.

## Alternatives considered

- Store everything in one events table: rejected because privacy, retention, and deletion boundaries would be unclear.
- Keep all data indefinitely: rejected because retention must be intentional and configurable.
- Use raw log files for future fine-tuning: rejected because logs are not a curated or reliably labeled dataset.

## Compliance

Database and settings phases must map every stored field to a data class, default, retention rule, export rule, and deletion rule. Relevant reports must verify that message bodies are absent from diagnostics by default.

## Related records

- EDR-002: Local-only privacy boundary
- EDR-005: Model observability and evaluation
- EDR-006: Learning process
- EDR-007: Learning capture and grammar analysis
- EDR-008: Study materials, reporting, and export
