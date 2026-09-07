# Phase 05: Vocabulary System

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/05-vocabulary-system.md`
- Dependencies: Completed Phase 04 and explicit Phase 05 approval
- Governing EDRs: EDR-001, EDR-002, EDR-003, EDR-004, EDR-006, EDR-007, EDR-008

## Objective

Build a deduplicated personal professional and IT vocabulary that preserves meanings, examples, usage history, and study progress.

## Approved scope

- Add migrations and services for canonical vocabulary items, senses, examples, occurrences, collocations, synonyms, and learning status.
- Normalize reliable inflected forms to a lemma without merging uncertain forms automatically.
- Store one canonical item per normalized lemma and language.
- Store parts of speech and context-dependent meanings as senses.
- Store every eligible dated use as an occurrence linked to its entry and sense.
- Exclude configurable stop words and low-value function words from automatic study suggestions.
- Support both `Words you used` and `Recommended professional vocabulary` with provenance.
- Generate a simple English definition, contextual Russian translation, pronunciation, original/corrected examples, additional English/Russian examples, collocations, synonyms, usage count, first/last use, and study status.
- Support New, Learning, Known, Ignored, and Needs review states as applicable.
- Add a dedicated Vocabulary view inside Study Materials and entry-level vocabulary suggestions beside Sentence Review.
- Add optional local macOS pronunciation playback without external speech services.

## Out of scope

- Full historical report generation and file export.
- Flashcard scheduling or spaced repetition unless separately approved later.
- External dictionaries, translation APIs, or pronunciation services.
- Vector embeddings and semantic similarity.

## Deliverables

- Vocabulary migrations and data relationships.
- Normalization and extraction pipeline.
- Sense, example, translation, and occurrence services.
- Vocabulary study UI and settings.
- Local pronunciation control where available.
- Phase report and vocabulary-processing learning notes.

## Acceptance criteria

- `restarted` and `restarting` map to `restart` when confidence is sufficient.
- Repeating a word adds occurrences and usage count without duplicate canonical items.
- Different meanings of `issue` remain distinct senses with their own translations and examples.
- Every displayed Russian translation is attached to the relevant sense.
- Automatic suggestions identify whether the word came from user usage or a model recommendation.
- Stop words are excluded according to configured rules.
- Uncertain normalization or sense matching is marked for review.
- Pronunciation playback remains local.
- Journal and per-entry storage controls are honored.

## Targeted test plan

- Canonical uniqueness and concurrent-insert tests.
- Lemma normalization and uncertainty fixtures.
- Sense separation and context tests.
- Occurrence counting and date-boundary tests.
- Stop-word and phrase-extraction tests.
- Russian translation/example field validation.
- Status-transition and provenance tests.
- Local pronunciation fallback tests.
- Existing Writer, Learning, Voice, and Observability smoke tests.

## Manual and privacy validation

- Use sanitized examples for `investigate`, `issue`, and an inflected form.
- Verify canonical cards, senses, translations, examples, and counts in SQLite and UI.
- Confirm no external dictionary or speech request occurs.

## Learning deliverables

- Explain tokenization, lemmatization, stop words, multiword expressions, canonical records, senses, occurrences, uniqueness, confidence, and local pronunciation in English.

## Completion report

Create `reports/phases/05-completion.md` with schema, normalization rules, ambiguous cases, tests, UI validation, privacy checks, and limitations.

## Rollback guidance

Document migration rollback, safe removal of derived vocabulary while preserving source entries, and recovery from an incorrect merge.

## Approval gate

Set Phase 05 to complete and stop at `awaiting_approval`. Do not begin Phase 06 without `Approve Phase 06`.
