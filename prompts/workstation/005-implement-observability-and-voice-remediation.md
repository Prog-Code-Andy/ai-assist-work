# Workstation Prompt 005: Implement Observability and Voice Remediation

## Authorization

- Application changes authorized: Only after all gates below pass
- Control repository changes authorized: Required remediation report, learning note, and remediation status only
- Full regression authorized: No
- Phase approval included: No
- Local remediation branch creation authorized: Yes, only as defined below
- Corrective approval phrase: `Approve Remediation 001`

## Mandatory gates

This prompt must stop without changes unless:

1. EDR-009 has status `Accepted` after explicit user review.
2. REM-001 has status `Approved` after explicit user review.
3. The current user message contains exactly `Approve Remediation 001`.
4. Both repository paths and existing worktree changes are understood and preserved.
5. Every substantive blocker identified by workstation prompts 004 and 006 is resolved. A historical `blocked` verdict caused only by the formerly Proposed statuses is resolved when gates 1 and 2 now pass; it does not require another repository-wide review.
6. The application repository is on `remediation/observability-voice-001`, or that branch can be created safely from the current Phase 05 HEAD without discarding or overwriting existing work.

## Prompt to copy into Claude Code

```text
Read and follow `CLAUDE.md` and `prompts/MASTER.md` from the control repository, but do not start or advance an implementation phase.

Use `.project/workstation-config.local.json` to resolve both repositories.

Validate every mandatory gate in:

prompts/workstation/005-implement-observability-and-voice-remediation.md

The exact corrective approval phrase is:

Approve Remediation 001

If any gate is missing, report the exact blocker and stop without changes.

When every gate passes, implement only:

docs/remediation/REM-001-observability-and-voice-reliability.md

Required execution rules:

1. Preserve existing uncommitted work and current application behavior outside the corrective scope.
2. If the application is still on `main`, create and switch to `remediation/observability-voice-001` from the current HEAD. If that branch already exists with conflicting history, stop. Do not commit, push, stash, reset, clean, or discard files.
3. Before edits, report the exact REM-001 file allowlist and exact two test commands. Do not rescan completed phases or the entire repository.
4. Implement the accepted EDR-009 dual-sink application diagnostic pipeline and browser error bridge without collecting user content or unrelated macOS activity.
5. Repair only the confirmed voice-recorder stop/error lifecycle described by REM-001. Do not implement the remaining Phase 04 feature scope.
6. Use the existing local `MediaRecorder → FastAPI → faster-whisper` architecture.
7. Run only the targeted tests and commands listed by REM-001. Do not run the full suite, `tests/test_voice_language_routing.py`, or unrelated completed-phase tests without separate explicit approval.
8. Before the real microphone check, stop and ask the user to participate. Run only one short sanitized recording after approval.
9. Create `reports/remediations/REM-001-completion.md` and `docs/learning/troubleshooting/REM-001-application-logs-and-async-recorder.md`.
10. Record exact changed files, test commands, intentionally omitted tests, privacy evidence, log locations, manual evidence, limitations, and rollback.
11. Update only remediation status records needed to mark REM-001 complete. Do not approve, complete, or advance Phase 06 or any other phase.
12. Give a concise completion summary and stop for user review. Do not commit or push unless the user separately requests it.
```

## Expected stop condition

Claude Code completes only REM-001, produces its evidence, leaves implementation phase state unchanged, and stops for review.
