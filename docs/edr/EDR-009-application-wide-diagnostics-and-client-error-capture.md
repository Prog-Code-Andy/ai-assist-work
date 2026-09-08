# EDR-009: Application-Wide Diagnostics and Client Error Capture

- Status: Proposed
- Date: 2026-09-08
- Supersedes: None
- Amends: EDR-004 and EDR-005

## Context

Phase 02 introduced structured JSONL diagnostics, but troubleshooting evidence showed an application-coverage gap. A browser-side `MediaRecorder.onstop` exception occurred before the transcription request reached FastAPI. The backend therefore had no request or exception to record, the JSONL log contained no matching event, and the UI remained stuck in `Transcribing...`.

A `.log` filename alone would not solve this problem. The application needs one explicit diagnostic event pipeline that captures privacy-safe events from every AI Assist Work component and writes both machine-readable and human-readable representations. This does not authorize collection of unrelated macOS activity or user content.

## Decision

AI Assist Work will maintain a unified, local-only application diagnostic pipeline with two synchronized sinks:

1. Rotating JSONL is the authoritative machine-readable diagnostic stream.
2. A rotating plain-text `application.log` is the human-readable operational view.

Both sinks represent the same sanitized application events and share an event identifier. A missing event in one sink is a diagnostics failure rather than an acceptable difference in coverage.

### Event coverage

The pipeline covers AI Assist Work components only:

- application startup, readiness, shutdown, configuration loading, and component availability;
- browser frontend errors, unhandled promise rejections, explicitly instrumented state-machine failures, failed local API calls, and supported media-recorder errors;
- FastAPI request lifecycle, validation failures, timeouts, cancellations, and unhandled exceptions;
- SQLite initialization, migrations, transaction failures, and retention jobs;
- Ollama client lifecycle and eligible model metrics governed by EDR-005;
- local transcription lifecycle, audio-format validation, cleanup, timeout, cancellation, and errors;
- report, export, diagnostic-bundle, and protected-deletion operations.

This decision does not authorize general macOS unified-log ingestion, browser-history collection, continuous process surveillance, or collection of unrelated application activity.

### Frontend-to-backend diagnostic bridge

Browser errors that occur before a normal feature request reaches FastAPI must be sent to a dedicated loopback-only diagnostic endpoint. The bridge must:

- accept an allowlisted, versioned event schema rather than arbitrary log strings;
- validate event type, severity, component, field sizes, and total payload size;
- apply rate limiting and duplicate suppression;
- attach or preserve session, operation, and correlation identifiers where available;
- reject message bodies, transcripts, raw audio, secrets, authorization data, cookies, and unrestricted browser state;
- sanitize stack locations and local paths before persistence;
- fail safely without recursively generating diagnostic traffic.

Targeted handlers include `window.error`, `unhandledrejection`, `MediaRecorder.onerror`, feature-level `try/catch`, and local fetch failures. Global handlers supplement explicit feature error handling; they do not replace it.

### Log levels and privacy

Three application log levels are supported:

- `Error`: failures and terminal error states.
- `Standard`: Error plus application/component lifecycle and important operations. This is the default.
- `Debug`: additional technical state transitions for a user-started, time-bounded troubleshooting session.

Debug mode must expire automatically, remain content-free, and show its active state in Application Control. No level may record prompts, rewritten responses, voice transcripts, raw audio, sensitive headers, secrets, or database record contents in diagnostic logs.

### Storage, rotation, and visibility

- Runtime logs live in the per-user application-data location, not either Git repository.
- JSONL and `application.log` rotate by configured size and age, share retention controls, and obey a combined disk cap.
- Each application start writes a sanitized startup marker so the user can verify that logging is active.
- Application Control → Logs and Diagnostics shows the resolved log directory, active file names, last event time, current log level, retention, disk use, and logging-health state.
- The UI provides `Open Logs Folder`, filtered preview, and diagnostic-bundle generation. It never uploads logs automatically.
- Console output may mirror warnings and errors for development, but terminal output is not the authoritative retained log.
- Claude-facing bundles remain bounded, redacted summaries with selected evidence, as required by EDR-005.

### Correlation and error model

Every eligible operation uses a correlation identifier across frontend events, FastAPI events, model/transcription activity, and terminal success/failure events. Events record timestamp, severity, component, event code, lifecycle status, safe error category, safe source location, and schema version. Exception text and stack traces are sanitized and bounded.

## Consequences

- Browser failures that occur before a feature API request can be diagnosed locally.
- Developers and the user gain a readable `.log` without losing the structured JSONL source required for filtering and analysis.
- Storage, redaction, schema validation, rate limiting, and health reporting become more complex.
- A log file can still be incomplete if a process terminates before flushing; stderr and startup recovery documentation remain necessary.
- Logging improves evidence but does not itself repair the failing feature.

## Alternatives considered

- Replace JSONL with plain text only: rejected because reliable filtering, grouping, redaction, and diagnostic bundles require structured events.
- Keep backend-only logs: rejected because browser failures may occur before any backend feature request.
- Capture all macOS and browser activity: rejected because it is disproportionate, privacy-invasive, and conflicts with EDR-002 and EDR-005.
- Store unrestricted JavaScript messages and stacks: rejected because they may contain user content, secrets, or local filesystem details.
- Rely only on DevTools Console: rejected because evidence disappears across sessions and is unavailable in normal user troubleshooting.

## Compliance

The corrective work package must prove dual-sink parity for representative events, frontend error delivery, redaction, rate limiting, rotation, retention, disk-cap behavior, logging-health visibility, and absence of user content. It must also demonstrate a controlled voice-recorder failure and recovery without running a full regression suite.

## Related records

- EDR-001: Phase-gated execution governance
- EDR-002: Local-only privacy boundary
- EDR-003: System architecture
- EDR-004: Data storage and retention
- EDR-005: Model observability and evaluation
- EDR-006: Learning process

