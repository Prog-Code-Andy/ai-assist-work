# REM-001: Observability Coverage and Voice Transcription Reliability

- Status: Proposed
- Date: 2026-09-08
- Exact approval phrase: `Approve Remediation 001`
- Requires: Accepted EDR-009
- Repairs: Phase 02 observability coverage and a confirmed existing voice defect within Phase 04 scope
- Phase advancement: None

## Objective

Make AI Assist Work failures locally observable in both structured and human-readable logs, then repair and verify the confirmed browser voice-recorder race without implementing the remaining Phase 04 feature scope.

## Confirmed defect

The current frontend uses `getUserMedia` and `MediaRecorder`. When Stop is selected, `stopAudioInfrastructure()` calls `mediaRecorder.stop()` and immediately sets the shared `mediaRecorder` reference to `null`. The asynchronous `onstop` callback later reads `mediaRecorder.mimeType`, throws a null-reference `TypeError`, never constructs the audio Blob, never calls `sendForTranscription`, and never sends `POST /api/voice/transcribe`. Because the exception occurs outside the existing transcription error handler and no recovery watchdog exists, the UI remains in `Transcribing...`.

The intended backend path already uses local FastAPI and local `faster-whisper`; no cloud speech service or architectural replacement is required.

## Approved corrective scope after acceptance

### A. Phase 02 observability coverage repair

- Inventory the existing JSONL logger, SQLite metrics, diagnostics UI, retention settings, and diagnostic bundle without repeating a full repository audit.
- Implement the EDR-009 unified diagnostic event schema and dual sinks: authoritative rotating JSONL plus synchronized rotating `application.log`.
- Record an application-start marker and expose logging-health state.
- Add a versioned, loopback-only, validated, rate-limited frontend diagnostic endpoint.
- Add sanitized frontend capture for global JavaScript errors, unhandled promise rejections, local fetch failures, `MediaRecorder.onerror`, and explicit feature state-machine failures.
- Add or complete backend exception/request lifecycle coverage for FastAPI, SQLite, Ollama integration, transcription, exports, and protected operations.
- Add correlation identifiers across eligible frontend and backend operations.
- Add Application Control fields for active files, resolved log directory, last event, log level, retention, disk use, health, filtered preview, `Open Logs Folder`, and bounded diagnostic bundle creation.
- Support Error, Standard, and time-bounded Debug levels. Standard is the default.
- Preserve existing compatible JSONL data and settings. Document any schema migration.

### B. Confirmed voice-recorder correction

- Preserve the recorder instance or MIME type in a local immutable value before asynchronous stop completion.
- Do not clear the shared recorder reference until the matching `onstop` lifecycle has completed, or stop referencing the shared value from that callback.
- Wrap the complete `onstop` body in `try/catch/finally` and always leave `Transcribing...` on failure.
- Add `MediaRecorder.onerror`, empty-Blob validation, duplicate-stop protection, and a bounded transcription timeout/cancellation path.
- Ensure Cancel does not submit audio and cleanup occurs once on success, failure, timeout, and cancellation.
- Emit sanitized correlated lifecycle/error events without audio or transcript content.
- Preserve the existing local `MediaRecorder → FastAPI → faster-whisper` architecture.

## Verified implementation boundary

Workstation prompts 004 and 006 confirmed the current application structure. Start with only this application-file allowlist; do not repeat a repository-wide audit.

| Application file | Authorized reason |
|---|---|
| `app/config.py` | Diagnostic level, file, rotation, retention, disk-cap, and bounded debug configuration |
| `app/diagnostics/jsonl_logger.py` | Unified sanitized event pipeline, JSONL/plain-text parity, rotation, retention, health, and redaction |
| `app/main.py` | Startup marker, FastAPI lifecycle/exception coverage, loopback diagnostic endpoint, health/preview/folder APIs, and correlation |
| `app/transcription.py` | Local transcription lifecycle/error events and safe terminal cleanup evidence |
| `app/db/repository.py` | SQLite initialization, migration, transaction-failure, retention, and protected-operation events |
| `app/static/app.js` | Frontend error bridge, recorder lifecycle repair, timeout/cancellation recovery, and diagnostic UI behavior |
| `app/static/index.html` | Logs and Diagnostics controls and status fields |

A new helper may be added only under `app/diagnostics/` when keeping it inside `jsonl_logger.py` would materially reduce cohesion or testability. Its purpose and need must be stated before creation. A directly required schema, settings, or test-support file outside this allowlist is a blocker: report the exact path and reason, then wait for user approval before editing it.

Control-repository writes are limited to:

- `reports/remediations/REM-001-completion.md`;
- `docs/learning/troubleshooting/REM-001-application-logs-and-async-recorder.md`;
- the REM-001 status fields after all acceptance criteria pass.

## Explicit exclusions

- Remaining Phase 04 features such as final language-mode UX, complete silence behavior, packaging, or broader voice redesign.
- Full macOS unified-log ingestion or unrelated process/application logging.
- Prompts, responses, transcripts, raw audio, secrets, cookies, authorization data, or database contents in diagnostics.
- Cloud logging, cloud speech, automatic upload, or external telemetry.
- Full application regression suite.
- Starting, approving, completing, or advancing Phase 03 or Phase 04.

## Deliverables

- Versioned diagnostic event schema and safe frontend bridge.
- Rotating JSONL and `application.log` with parity and health checks.
- Logs and Diagnostics UI completion.
- Corrected voice-recorder stop/error lifecycle.
- Targeted automated evidence and one short sanitized manual voice validation.
- `reports/remediations/REM-001-completion.md`.
- `docs/learning/troubleshooting/REM-001-application-logs-and-async-recorder.md`.

## Targeted test plan

Run only new or directly affected tests. The approved existing test-file allowlist is:

- `tests/test_jsonl_logger.py`;
- `tests/test_feedback_and_diagnostics_api.py`;
- `tests/test_voice_api.py`;
- `tests/test_voice_cleanup.py`;
- `tests/test_transcription.py`.

Use these two bounded commands from the application repository unless the project's existing documented environment requires the same selectors through its established runner:

```text
python -m pytest -q tests/test_jsonl_logger.py tests/test_feedback_and_diagnostics_api.py
python -m pytest -q tests/test_voice_api.py tests/test_voice_cleanup.py tests/test_transcription.py
```

New tests must be placed in one of those five files. Do not create a new test framework. Before running either command, list its selected test node IDs; if a listed file does not exist or collection fails, stop and report the exact mismatch rather than broadening test discovery.

The selected tests must cover:

- event schema acceptance/rejection, size bounds, rate limiting, duplicate suppression, and recursive-failure prevention;
- JSONL/plain-log parity, rotation, retention, disk cap, startup marker, and logging-health tests;
- redaction tests proving exclusion of message bodies, transcripts, audio, secrets, sensitive headers, cookies, and unsafe paths/stacks;
- frontend `window.error`, `unhandledrejection`, fetch failure, and `MediaRecorder.onerror` bridge tests;
- voice Stop preserves MIME type, produces one non-empty Blob, and sends exactly one transcription POST;
- voice error, empty Blob, timeout, duplicate Stop, and Cancel restore the correct UI and cleanup state;
- one directly related FastAPI transcription endpoint test with sanitized fixture data;
- one existing Writer smoke test by exact node ID only if non-voice Writer behavior or shared request infrastructure changed; identify it with `rg`, do not collect or run an entire additional test file;
- one manual 3–5 second browser recording confirming a POST, local transcription review, log correlation, and cleanup.

Do not run `tests/test_voice_language_routing.py`, unrelated completed-phase tests, an entire additional smoke-test file, or the full suite unless the implementation actually changes the corresponding behavior and the user separately approves the precise expansion. Record exact commands, selected tests, intentionally omitted tests, and any approved scope expansion.

## Acceptance criteria

- A controlled frontend exception appears in both JSONL and `application.log` with the same event and correlation identifiers.
- The same event is visible through Application Control without exposing user content.
- Every application start creates a visible logging marker and current logging-health state.
- Rotation, retention, combined disk cap, redaction, rate limiting, and bundle bounds pass targeted tests.
- Stopping a voice recording creates a non-empty Blob and exactly one `POST /api/voice/transcribe`.
- Successful local transcription enters review state; failure, timeout, empty audio, and cancellation never leave the UI stuck.
- Temporary audio cleanup follows the accepted raw-audio policy.
- No external request, cloud speech, external telemetry, or broad macOS logging is introduced.
- Phase status is not advanced and Phase 04 is not marked complete.

## Completion and approval gate

After completing the work, create the remediation report and learning note, summarize changed files and targeted tests, state whether any acceptance criterion remains blocked, and stop. The user must review the remediation independently from the next implementation phase approval.
