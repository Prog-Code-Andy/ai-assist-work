# Phase Prompt 01: SQLite, Settings, and Privacy Foundation

Execute this prompt only through `prompts/MASTER.md` after Phase 00 is complete and `Approve Phase 01` is validated.

## Required specification

Read and implement only `docs/phases/01-sqlite-settings-and-privacy.md`.

## Execution instructions

1. Use the Phase 00 audit rather than assuming the application structure.
2. Add minimal, versioned SQLite and settings foundations that fit the existing codebase.
3. Implement data classification, local configuration, loopback boundaries, Journal consent, per-entry save foundation, retention foundation, and protected data-management boundaries.
4. Preserve the Writer experience and do not implement future grammar, telemetry, voice, vocabulary, reporting, or packaging features.
5. Test migrations and the full content-persistence consent matrix with temporary data.
6. Verify no external runtime dependency or sensitive default is introduced.
7. Produce the report and English learning documentation, update state, request `Approve Phase 02`, and stop.
