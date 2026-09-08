# Workstation Prompt 004: Review Observability and Voice Remediation

## Authorization

- Application changes authorized: No
- Control repository changes authorized: No
- Test execution authorized: No
- EDR or remediation approval included: No
- Expected result: Read-only readiness and conflict report

## Prompt to copy into Claude Code

```text
Do not modify either repository.
Do not run tests.
Do not approve or start an implementation phase or corrective package.

Use `.project/workstation-config.local.json` from the control repository to resolve both repositories.

Read completely:

- `CLAUDE.md`
- `docs/edr/EDR-009-application-wide-diagnostics-and-client-error-capture.md`
- `docs/remediation/README.md`
- `docs/remediation/REM-001-observability-and-voice-reliability.md`
- `docs/edr/EDR-002-local-only-privacy-boundary.md`
- `docs/edr/EDR-004-data-storage-and-retention.md`
- `docs/edr/EDR-005-model-observability-and-evaluation.md`
- `docs/phases/02-observability-logs-and-evaluation.md`
- `docs/phases/04-local-voice-and-macos.md`

Perform a focused read-only inspection of only the relevant existing application logger, diagnostics UI, frontend error handling, voice recorder, transcription endpoint, settings, and directly related tests.

Return only:

1. Current project phase and approval state as found on this workstation.
2. Existing JSONL log location, schema, rotation, retention, event sources, and UI access.
3. Whether a human-readable `.log` exists and where.
4. Which AI Assist Work components currently emit events and which do not.
5. Whether browser errors can reach the backend diagnostic store.
6. Confirmation or correction of the documented `mediaRecorder` stop race with exact file and line evidence.
7. Proposed files that REM-001 would change or add.
8. Exact targeted tests that should run; explicitly identify tests that should not run.
9. Any conflict with an Accepted EDR, current phase work, existing uncommitted changes, or privacy rule.
10. Readiness verdict: `ready for EDR review`, `ready after listed correction`, or `blocked`.

Do not implement recommendations. Stop after the report.
```

## Expected stop condition

Claude Code returns the read-only report. The user reviews EDR-009 and REM-001 before accepting either record or authorizing implementation.

