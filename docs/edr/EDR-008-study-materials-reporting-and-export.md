# EDR-008: Study Materials, Reporting, Printing, and Export

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

Stored learning entries become useful only when the user can review and export them by period and topic. The user needs distinct views of vocabulary and sentence-level grammar mistakes, weekly or monthly study material, unique word lists, examples with Russian translations, printable output, and editable tabular exports. These controls must not overcrowd the Writer interface or be confused with technical diagnostics.

## Decision

Create a dedicated `Study Materials` workspace backed by deterministic SQLite queries and optional local-LLM explanations.

### Information architecture

The primary navigation is:

```text
Writer | Study Materials | Analytics | Settings
```

`Study Materials` contains:

```text
Vocabulary | Sentences & Grammar | Report Builder | Downloads
```

The Writer page shows only entry-specific learning results and a link to the corresponding stored material. Global storage, extraction, reporting, and retention configuration belongs in the separate Application Control settings described below.

### Report types

The system supports three related report types generated from the same stored learning data:

1. `Vocabulary Report`: unique words and expressions selected for study.
2. `Sentences & Grammar Report`: original sentences, corrections, rules, explanations, and examples.
3. `Learning Summary`: aggregated weekly, two-week, monthly, or custom-period progress combining vocabulary and grammar trends.

The user may generate either individual reports or a combined learning package. Separate report types remain available because vocabulary study and grammar review have different table structures.

The exact visible fields, nesting, example output, export columns, identifiers, empty states, and sorting rules are defined by the proposed [Report Output Contract v1](../contracts/report-output-contract-v1.md). EDR-008 governs the product decision; the versioned contract governs the required output structure.

### Common filters

The report builder supports:

- Today, Last 7 Days, Last 14 Days, Current Month, Previous Month, All Time, and Custom Range;
- typed, voice, or all input sources;
- writing mode and model;
- grammar category and rule;
- vocabulary context, part of speech, and learning status;
- New, Reviewing, Understood, Known, or Ignored material;
- minimum usage count;
- include or exclude low-confidence findings;
- search text;
- ascending or descending sorting;
- a `Unique vocabulary only` checkbox, enabled by default.

A custom date range covers non-standard periods such as ten days or one and a half weeks.

### Vocabulary uniqueness and occurrences

Vocabulary storage and reporting distinguish identity from usage:

```text
Canonical vocabulary item: investigate
├── sense: examine a problem
│   ├── occurrence: 2026-09-01
│   └── occurrence: 2026-09-04
└── sense: conduct a formal inquiry
    └── occurrence: 2026-09-05
```

Rules:

1. Inflected forms such as `investigated` and `investigating` map to the lemma `investigate` when the normalization is reliable.
2. A canonical vocabulary item is unique by normalized lemma and language.
3. Different parts of speech and context-dependent meanings are stored as separate senses, not duplicate vocabulary items.
4. Repeated uses create occurrences with date, source entry, and context; they do not create duplicate cards.
5. The default report groups by vocabulary item and sense and displays a usage count.
6. When `Unique vocabulary only` is disabled, the report may display individual occurrences for detailed history.
7. Uncertain normalization is marked for review rather than merged automatically.

### Vocabulary report columns

The default vocabulary table contains:

- word or expression;
- lemma;
- part of speech;
- pronunciation;
- simple English definition;
- context-specific Russian translation;
- context or domain;
- original user example when available;
- corrected example;
- an additional natural English example;
- Russian translation of the additional example;
- useful collocations;
- usage count;
- first and last occurrence;
- learning status.

Because one word can have several meanings, the Russian translation and examples belong to a sense, not only to the canonical word.

### Sentences and grammar report columns

The default sentence table contains:

- date and input source;
- original typed sentence or reviewed voice transcript;
- corrected sentence;
- optional user-edited final sentence;
- highlighted incorrect and corrected fragments;
- grammar category and rule;
- junior-friendly English explanation;
- explanation of the relevant timeline or context;
- correct example;
- Russian translation of the example;
- confidence or `Needs review` state;
- learning status.

The database preserves one sentence and its linked findings without duplicating source records. The user-facing historical report intentionally uses one row per finding and repeats the complete Original and Corrected sentences in every row so each row remains understandable after Excel/CSV filtering. This presentation duplication must not create duplicate database records.

### Generation pipeline

Reports use the following order:

```text
User filters
→ SQLite selects and aggregates authoritative records
→ Deterministic tables and counts are created
→ Optional local LLM writes summary, explanations, and exercises
→ User previews the result
→ Local export is generated
```

The LLM must not invent table rows, counts, dates, or user errors. Generated narrative must remain distinguishable from database-derived facts. Weekly and monthly summaries may include frequent error categories, recurring tense problems, vocabulary growth, progress compared with the previous period, and suggested exercises.

Initial report generation is manual through a `Generate Report` button. Automatic schedules are deferred until report accuracy and privacy behavior are validated.

### Export and printing

Supported local exports are:

- self-contained HTML with embedded styles and no CDN dependencies;
- print-optimized HTML for browser `Print` or `Save as PDF`;
- locally generated PDF only if a later implementation phase validates a maintainable local renderer;
- UTF-8 CSV for editable vocabulary or sentence tables;
- JSON for structured backup or approved downstream processing;
- approved JSONL learning dataset only under the separate future-training policy.

Vocabulary and sentence report exports are separate because their columns differ. The default sentence-learning CSV is a flat, self-contained table with one row per finding; full Original and Corrected sentences repeat in each row intentionally. An optional normalized multi-file export may be provided separately for backup or machine processing. A combined package may contain the selected report files plus an HTML summary.

Every export provides a preview and records local metadata such as report type, filters, generation time, row counts, format, and local file location. Exported files are copies: editing or deleting them does not silently modify SQLite data.

### Application Control settings

The separate Settings area includes these categories:

1. `Privacy & Learning Journal`: global opt-in, per-entry default, stored content, and retention.
2. `Learning & Vocabulary`: grammar categories, explanation level, examples, Russian translations, stop words, normalization review, and vocabulary status rules.
3. `Reports & Export`: default period, uniqueness behavior, included columns, export format, local destination, and report history.
4. `Voice & macOS Permissions`: language mode, transcription review, silence timeout, and raw-audio policy.
5. `Logs & Diagnostics`: technical logging, rotation, diagnostic bundles, and local retention.
6. `Performance`: model metrics and system sampling.
7. `Data Management`: review, export, backup, and protected deletion of source records.

The settings interface is separate from `Study Materials`: settings control behavior, while Study Materials filters, previews, studies, and exports actual learning records.

## Consequences

- The user can create focused word lists, grammar reviews, or combined learning packages.
- Canonical vocabulary remains unique without losing frequency and context history.
- SQL-derived facts remain auditable while local LLM summaries add teaching value.
- Multiple export formats support printing, spreadsheet filtering, and structured reuse.
- Report generation, local file management, and privacy validation add implementation complexity.
- Direct PDF generation is not assumed until a local rendering approach is validated; print-ready HTML remains the reliable baseline.

## Alternatives considered

- One large report containing everything: rejected because vocabulary and sentence learning require different tables and filters.
- A linked two-file CSV as the default sentence study export: rejected because filtering one file would separate an error from its full Original, Corrected, rule, and explanation context.
- Separate duplicate word rows for each use: rejected because it produces noisy study lists and inconsistent status.
- Keep only one word row and discard repeated uses: rejected because usage frequency and historical context are valuable.
- Ask the LLM to generate the entire report from raw text: rejected because counts and historical facts must come from SQLite.
- Put all controls on the Writer page: rejected because writing, study, infrastructure, and privacy controls have different purposes.
- Generate only PDF: rejected because HTML and CSV are easier to inspect, filter, edit, and reproduce.

## Compliance

Roadmap and phase specifications must define canonical vocabulary constraints, occurrence relationships, sentence-to-finding relationships, report queries, filtering semantics, generated-content labeling, preview behavior, export encoding, print layout, local file handling, and tests for deduplication and date boundaries.

They must implement the accepted version of `docs/contracts/report-output-contract-v1.md` without silently removing required fields.

## Related records

- EDR-002: Local-only privacy boundary
- EDR-003: Workstation application architecture
- EDR-004: Data storage and retention
- EDR-005: Model observability and evaluation
- EDR-006: Learning process
- EDR-007: Learning capture and grammar analysis
