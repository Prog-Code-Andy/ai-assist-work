# AI Assist Work Product Roadmap

- Status: Approved
- Version: 1.0
- Date: 2026-09-06
- Approved: 2026-09-07
- Governing decisions: EDR-001 through EDR-008
- Output contract: Report Output Contract v1.0

## Product outcome

Build a private, local-first macOS writing and English-learning application that:

- rewrites rough English into concise professional business English;
- accepts typed or locally transcribed voice input;
- explains grammar and vocabulary using the user's own eligible examples;
- generates study materials and printable/exportable reports;
- measures local model behavior, tokens, latency, CPU, memory, and related performance factors;
- stores only explicitly permitted content and never transmits content or telemetry externally;
- teaches the user how the application, tools, integrations, and troubleshooting process work.

## Delivery principles

1. Accepted EDRs are mandatory and immutable.
2. Preserve the workstation's existing working application before enhancement.
3. Execute one explicitly approved phase at a time.
4. Use targeted and directly related tests after each phase; full regression requires a defined gate or explicit approval.
5. Treat privacy validation, learning documentation, and completion reporting as deliverables.
6. Store factual report data through deterministic database queries. Label optional LLM-generated guidance.
7. Prefer simple, local, inspectable components over unnecessary infrastructure.
8. Block rather than guess when project state, approval, paths, or prerequisites are inconsistent.

## Product boundaries

### Included

- Local browser/PWA interface and FastAPI application service.
- Ollama-native local model integration and streaming.
- SQLite storage, migrations, retention, export, and protected deletion.
- Writer modes and current-entry Sentence Review.
- Local grammar analysis, vocabulary, and learning journal.
- Local microphone capture and speech-to-text.
- Observability, diagnostic bundles, model evaluation, and analytics.
- Study Materials, HTML/PDF-ready, CSV, and JSON output.
- macOS launch and packaging experience.
- Full-text search and future-dataset readiness after the core product is stable.

### Excluded unless a later EDR approves them

- External AI, speech, analytics, telemetry, database, font, or CDN services.
- Automatic upload of logs, reports, prompts, or training data.
- Automatic control, termination, or reprioritization of unrelated workstation processes.
- Raw-audio retention by default.
- Unreviewed fine-tuning or training on all collected content.
- Automatic report scheduling before manual report accuracy and privacy are validated.

## Dependency sequence

```text
Phase 00 Audit and Protection
  ↓
Phase 01 SQLite, Settings, and Privacy Foundation
  ↓
Phase 02 Observability, Logs, and Model Evaluation
  ↓
Phase 03 Learning Capture, Grammar, and Sentence Review
  ↓
Phase 04 Local Voice Input and macOS Permissions
  ↓
Phase 05 Vocabulary System
  ↓
Phase 06 Study Materials, Reports, and Export
  ↓
Phase 07 PWA and macOS Packaging
  ↓
Phase 08 Search and Future Dataset Readiness
```

Later phases may depend on earlier schemas, but no phase may implement a future phase's user-facing functionality early.

## Phase roadmap

### Phase 00 — Audit and Project Protection

Establish the factual workstation baseline, map the existing application, protect recoverability, verify current privacy boundaries, and record performance before modifying behavior.

Outcome: a reviewed audit and safe implementation baseline.

### Phase 01 — SQLite, Settings, and Privacy Foundation

Create migrations and repositories for settings, sessions, eligible content, retention, and data classification. Establish the separate Application Control shell and visible Learning Journal consent state.

Outcome: durable local foundations without yet implementing the complete learning experience.

### Phase 02 — Observability, Logs, and Model Evaluation

Add structured rotating diagnostics, Ollama token/timing capture, request-scoped system sampling, feedback foundations, analytics filters, and local diagnostic bundles.

Outcome: model and workstation behavior can be measured without logging message bodies by default.

### Phase 03 — Learning Capture, Grammar, and Sentence Review

Implement opt-in learning capture, original/corrected/final linkage, structured grammar findings, uncertainty handling, per-entry controls, and the single-card current-entry Sentence Review interface.

Outcome: eligible writing requests become explainable, reviewable learning records.

### Phase 04 — Local Voice Input and macOS Permissions

Add explicit microphone capture, visible recording state, silence timeout, local speech-to-text, raw/reviewed transcript handling, English/Russian/Auto modes, and temporary-audio cleanup.

Outcome: voice input produces reviewed text and learning records without retaining raw audio by default.

### Phase 05 — Vocabulary System

Implement extraction, stop-word filtering, lemmatization, canonical vocabulary identity, senses, occurrences, contextual Russian translations, examples, collocations, usage counts, and learning states.

Outcome: a deduplicated personal professional/IT vocabulary with preserved context history.

### Phase 06 — Study Materials, Reports, and Export

Build the Study Materials workspace, vocabulary and flat sentence/grammar reports, filters, previews, report history, self-contained HTML, print-to-PDF layout, CSV, JSON, and combined learning summaries exactly matching Report Output Contract v1.0.

Outcome: the user can generate, study, filter, print, and export trustworthy learning material.

### Phase 07 — PWA and macOS Packaging

Add local application identity, icons, installable PWA behavior where supported, and a macOS launcher that starts one backend instance, checks Ollama and health, and opens the local interface.

Outcome: a reliable one-action local launch experience with documented macOS behavior.

### Phase 08 — Search and Future Dataset Readiness

Add SQLite full-text search, evaluate optional local embeddings for similarity search, support explicit training-eligibility review and approved JSONL export, and produce a fine-tuning feasibility assessment without performing training.

Outcome: historical learning data is searchable and a privacy-reviewed future improvement path is documented.

## Cross-cutting acceptance themes

Every phase must address the relevant themes:

- local-only network behavior;
- data classification and consent;
- migrations and rollback;
- accessibility and understandable errors;
- targeted automated tests and manual validation;
- performance impact;
- sensitive-data redaction;
- learning documentation in English;
- short completion report;
- synchronized machine and human status;
- explicit stop before the next phase.

## Release checkpoints

### Foundation checkpoint — after Phase 02

The original writer remains usable, data controls are visible, migrations are reversible, and technical behavior is measurable.

### Learning checkpoint — after Phase 05

Typed and voice inputs can produce grammar and vocabulary records with correct privacy behavior and traceable examples.

### Product checkpoint — after Phase 07

Study materials, exports, and local launch experience work end to end.

### Future-readiness checkpoint — after Phase 08

Search and approved dataset export are validated; fine-tuning remains a separate future decision.

## Success measures

- No external content or telemetry transmission is detected during validation.
- Thinking remains off by default and is enabled only by explicit selection.
- Stored content exactly follows Learning Journal and per-entry controls.
- Token and timing metrics preserve their source and units.
- Performance reports distinguish model work from concurrent system load where evidence permits.
- Vocabulary cards remain canonical while occurrences preserve usage history.
- Sentence CSV rows remain self-contained after Excel filtering.
- HTML reports open offline and print clearly.
- Every completed phase has tests, a report, learning notes, and an approval boundary.

## Change control

Version 1.0 was approved on 2026-09-07. A material change that conflicts with an accepted EDR requires a new EDR first; other roadmap changes require a versioned roadmap update and user approval.
