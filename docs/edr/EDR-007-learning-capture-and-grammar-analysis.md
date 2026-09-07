# EDR-007: Learning Capture and Grammar-Analysis Workflow

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

The user may type rough English or dictate it through a microphone. The application must create a professional response while also helping the user understand recurring grammar and vocabulary problems. Raw audio does not need to be retained, but an eligible transcript, corrections, rules, and examples must remain connected for future study.

Saving every work message without informed consent creates a privacy risk. Requiring a manual save for every useful example creates friction and causes learning records to be lost.

## Decision

Use an explicit opt-in Learning Journal with visible per-entry control.

### Capture flow

```text
Typed text or temporary voice recording
→ Voice transcription when applicable
→ User may review the input/transcript
→ Local model produces professional rewrite
→ Local grammar analysis produces structured findings
→ User sees correction and learning explanation
→ Eligible Journal policy saves the linked learning record
```

### Saving policy

1. Learning Journal is off on first launch.
2. While it is off, processing is transient and message content is not persisted after the active session, except temporary technical state required to complete the request.
3. Enabling Learning Journal requires an explicit local-storage confirmation.
4. Once enabled, each successfully completed generation is saved automatically for learning by default.
5. The UI clearly shows `Saved locally for learning` and provides an immediate `Remove from learning` action.
6. Before generation, the user may turn off `Save this entry for learning` for a sensitive message.
7. After generation, an unsaved entry may be added with `Save for learning`.
8. Copying the result does not independently trigger storage; the visible Journal setting determines storage behavior.
9. Disabling Learning Journal stops future automatic saves but does not silently delete existing records.
10. Existing learning data has separate review, export, and protected deletion controls.

### Stored learning record

An eligible entry links:

- input source: typed or voice;
- original typed text or reviewed voice transcript;
- professional model rewrite;
- optional user-edited final version;
- model, prompt version, writing mode, and timestamp;
- zero or more structured grammar findings;
- zero or more vocabulary findings;
- user feedback and learning status.

Raw audio is not part of this record unless a future explicit setting and EDR permit it.

### Grammar finding

Each finding must support:

- original incorrect fragment;
- corrected fragment;
- category, such as tense, article, preposition, agreement, word order, spelling, or punctuation;
- specific rule name when the model can identify it reliably;
- junior-friendly explanation of why the original is wrong or unsuitable;
- explanation of why the correction fits the intended timeline or context;
- at least one correct example and its Russian translation;
- optional contrast example showing the different meaning of the original grammar;
- confidence or `needs review` state so uncertain model analysis is not presented as fact;
- learning status such as New, Reviewing, Understood, or Ignored.

For example, the application must not merely replace Present Perfect with Past Perfect. It must explain the time relationship that makes Past Perfect appropriate and show a correct contextual example. If both forms could be valid depending on meaning, the finding must explain the ambiguity rather than claim one universal answer.

### User interface

The rewritten result remains the primary output. A separate learning panel displays:

- `What changed`;
- `Grammar and why`;
- `Better examples`;
- `Vocabulary`;
- `Save for learning` state;
- links to related recurring mistakes.

Learning controls must not be mixed with system diagnostics and infrastructure settings. Global Journal, retention, export, and deletion controls belong in Application Control; entry-specific learning explanations remain beside the writing result.

## Consequences

- Useful learning records are captured without requiring repetitive manual saving.
- Sensitive messages can be excluded before processing is persisted and removed afterward.
- Voice learning works without retaining raw audio.
- Grammar explanations become queryable for weekly and monthly reports.
- The local model may produce uncertain grammar analysis, so confidence, ambiguity handling, and later review are required.
- Additional structured analysis may increase generation time and token usage; the implementation must measure this cost.

## Alternatives considered

- Automatically save every message without opt-in: rejected because work content may be sensitive.
- Manual save only: rejected because useful examples would frequently be missed.
- Save only the rewritten result: rejected because learning requires the original, correction, rule, and context.
- Store raw audio for learning: rejected as the default because the transcript provides the required language evidence with less privacy risk.

## Compliance

Roadmap and phase specifications must define the database relationships, visible Journal state, save/remove behavior, structured grammar schema, uncertainty handling, retention, deletion, and tests proving that unsaved content is not persisted.

## Related records

- EDR-002: Local-only privacy boundary
- EDR-004: Data storage and retention
- EDR-005: Model observability and evaluation
- EDR-006: Learning process
- EDR-008: Study materials, reporting, and export
