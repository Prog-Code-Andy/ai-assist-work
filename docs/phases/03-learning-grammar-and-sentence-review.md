# Phase 03: Learning Capture, Grammar, and Sentence Review

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/03-learning-grammar-and-sentence-review.md`
- Dependencies: Completed Phase 02 and explicit Phase 03 approval
- Governing EDRs: EDR-001, EDR-002, EDR-003, EDR-004, EDR-006, EDR-007

## Objective

Turn eligible typed writing requests into structured, explainable English-learning records and present the current sentence and all corrections in one coherent UI.

## Approved scope

- Add schema and services for grammar findings, feedback, learning states, prompt versions, and user-edited final text.
- Implement the EDR-007 capture and saving matrix, including `Save this entry for learning` and `Remove from learning`.
- Generate structured local grammar analysis separately from the primary professional rewrite when appropriate.
- Validate structured model output before persistence and preserve raw model-analysis errors only in privacy-safe diagnostics.
- Record original, corrected, and optional user-final text with stable relationships.
- Support category, rule, incorrect fragment, corrected fragment, English explanation, contextual reason, example, Russian example translation, confidence, and review state.
- Distinguish a certain correction from ambiguity or `Needs review`.
- Add the current-entry Sentence Review card: Original and Corrected remain together, matching findings share visual identifiers, and selecting a finding highlights both fragments without navigating away.
- Use a stacked form of the same card on narrow screens.
- Add Good/Bad feedback and optional reason for the rewrite and analysis.
- Add the Learning settings required by accepted EDRs.

## Out of scope

- Voice recording and speech recognition.
- Vocabulary extraction and study cards.
- Historical report/export workspace.
- Fine-tuning or dataset export.

## Deliverables

- Grammar and learning migrations.
- Versioned structured-analysis contract.
- Local analysis service and validation.
- Journal/per-entry storage workflow.
- Sentence Review UI and accessible interaction.
- Feedback workflow.
- Phase report and learning notes.

## Acceptance criteria

- One input, rewrite, final edit, and multiple findings remain correctly linked.
- Journal OFF and per-entry OFF never persist learning content.
- Remove-from-learning deletes or tombstones the intended learning record according to the approved data policy without damaging diagnostics.
- Original and Corrected are visible in the same Sentence Review card.
- Each finding identifies the exact phrase in both versions and displays its rule and explanation in the same view.
- Four findings produce four selectable corrections without four duplicated current-entry cards.
- Ambiguous grammar is not presented as universally wrong.
- Present/Past/Perfect explanations describe the relevant timeline.
- Model output failing schema validation does not corrupt stored learning data.

## Targeted test plan

- Capture-policy matrix and persistence tests.
- Multi-finding relational integrity tests.
- Structured-output validation and malformed-model-response tests.
- Grammar-category, confidence, ambiguity, and example fields.
- Sentence Review keyboard, selection, responsive, and accessibility behavior.
- Removal and feedback tests.
- Token/performance measurement of the additional analysis call.
- Existing Writer, Settings, and Observability smoke tests.

## Manual and privacy validation

- Review a sanitized sentence containing tense, article, and agreement issues.
- Verify all corrections stay linked in one view.
- Verify saved/unsaved indicators against actual SQLite contents.
- Verify no learning content enters diagnostic JSONL by default.

## Learning deliverables

- Explain structured LLM output, schema validation, one-to-many relationships, confidence, ambiguity, prompt versioning, and the Sentence Review interaction in English.
- Walk through one finding from user input to SQLite and UI.

## Completion report

Create `reports/phases/03-completion.md` with schema, prompt version, storage matrix, UI evidence, test results, token overhead, privacy validation, and limitations.

## Rollback guidance

Document disabling analysis independently from rewriting, rolling back migrations, and preserving previously valid entries.

## Approval gate

Set Phase 03 to complete and stop at `awaiting_approval`. Do not begin Phase 04 without `Approve Phase 04`.
