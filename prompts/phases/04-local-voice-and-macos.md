# Phase Prompt 04: Local Voice Input and macOS Permissions

Execute this prompt only through `prompts/MASTER.md` after Phase 03 is complete and `Approve Phase 04` is validated.

## Required specification

Read and implement only `docs/phases/04-local-voice-and-macos.md`.

## Execution instructions

1. Validate the real workstation/browser permission model and select a fully local speech engine with documented evidence.
2. Do not download a dependency, model, or tool without the user's authorization when required.
3. Implement explicit recording, visible status, stop/cancel, silence timeout, maximum duration, and transcript review.
4. Delete temporary raw audio on every defined terminal path when retention is off.
5. Route English, Russian, and Auto modes correctly without labeling Russian speech as incorrect English grammar.
6. Surface uncertain transcription before grammar analysis.
7. Do not package the application or enable background recording.
8. Complete one short real microphone validation with user participation when available.
9. Produce the report and English learning documentation, update state, request `Approve Phase 05`, and stop.
