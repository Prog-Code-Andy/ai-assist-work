# Phase 04: Local Voice Input and macOS Permissions

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/04-local-voice-and-macos.md`
- Dependencies: Completed Phase 03 and explicit Phase 04 approval
- Governing EDRs: EDR-001, EDR-002, EDR-003, EDR-004, EDR-006, EDR-007

## Objective

Add transparent, local microphone capture and speech transcription that feeds the existing Writer and learning workflow without retaining raw audio by default.

## Approved scope

- Validate browser and macOS microphone permission behavior on the actual workstation.
- Select and document a fully local speech-to-text engine after comparing compatibility, accuracy, resource use, maintenance, and existing environment constraints.
- Add explicit Start, Stop, and Cancel controls, visible recording state, duration, and local-transcription status.
- Add configurable silence timeout with 15 seconds as the proposed default and a configurable maximum recording duration.
- Stop recording on silence, user action, cancellation, permission loss, or maximum duration.
- Keep temporary raw audio only as long as needed and verify cleanup on success, failure, cancellation, and application restart recovery.
- Support `Speak English → Improve English`, `Speak Russian → Professional English`, and `Auto Detect` modes.
- Preserve raw and reviewed transcript distinction where available, while storing only policy-eligible transcript fields.
- Show transcript for user review before generation. Automatic generation after transcription remains OFF by default.
- Prevent uncertain speech-recognition tokens from automatically becoming confident grammar mistakes.
- Add Voice and macOS Permissions settings and troubleshooting guidance.

## Out of scope

- Cloud speech services.
- Background or always-on recording.
- Raw-audio retention by default.
- Automatic generation without transcript review by default.
- Treating Russian speech as incorrect English grammar.
- Application packaging and launcher.

## Deliverables

- Local speech-engine decision note subordinate to accepted EDRs.
- Permission and recording lifecycle.
- Silence and duration controls.
- Transcription review and Writer integration.
- Temporary-audio cleanup and recovery.
- Voice settings and error states.
- Phase report and macOS/voice learning notes.

## Acceptance criteria

- Recording never begins without user action and visible indication.
- Denied or policy-managed permission produces an understandable, actionable message.
- Silence timeout is configurable and defaults to the approved value.
- Raw audio is removed on all defined terminal paths when retention is off.
- Reviewed transcript reaches the existing writing pipeline without bypassing Journal controls.
- English voice input can produce grammar analysis; Russian voice input produces translation/vocabulary assistance without false English-error claims.
- Low-confidence transcription is surfaced for review.
- No audio or transcript is sent outside the workstation.

## Targeted test plan

- Recording state-machine tests.
- Silence, maximum-duration, cancel, and permission-error tests.
- Temporary-file lifecycle and startup cleanup tests.
- Language-mode routing tests.
- Transcript review and storage-policy tests.
- Mocked speech-engine error and confidence tests.
- One short real local microphone test with explicit user participation.
- Existing Writer/Learning/Observability smoke tests.

## Manual and privacy validation

- The user performs macOS/browser permission steps and observes the result.
- Inspect temporary storage before, during, and after transcription.
- Verify network activity remains local.
- Verify a transcript is not saved when Journal/per-entry saving is off.

## Learning deliverables

- Explain browser microphone APIs, macOS permissions, audio lifecycle, silence detection, local speech-to-text, language modes, confidence, and cleanup in English.
- Document actual permission screens and troubleshooting without recording sensitive content.

## Completion report

Create `reports/phases/04-completion.md` with engine selection evidence, permission behavior, cleanup evidence, accuracy limitations, performance cost, tests, and privacy validation.

## Rollback guidance

Document how to disable voice independently, revoke permissions, remove the local speech component if separately installed, and verify no temporary audio remains.

## Approval gate

Set Phase 04 to complete and stop at `awaiting_approval`. Do not begin Phase 05 without `Approve Phase 05`.
