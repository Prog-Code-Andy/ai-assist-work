# EDR-002: Local-Only Privacy and Network Boundary

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

The application may process work-related text, voice transcripts, learning history, and diagnostic information on a managed Mac. These records may contain confidential or personally sensitive information.

## Decision

AI Assist Work is local-only by design.

1. The application server and Ollama bind to loopback addresses only.
2. Browser-to-backend and backend-to-Ollama calls are local API calls and are permitted.
3. External AI APIs, cloud speech services, remote databases, external analytics or telemetry, CDNs, remote fonts, and automatic uploads are prohibited.
4. Model inference, speech transcription, storage, reports, and analysis run locally.
5. The user must explicitly enable storage of message content through the Learning Journal.
6. Raw audio storage is off by default. Temporary audio is deleted after successful transcription or cancellation. The resulting text transcript is a separate data object and may be stored only under the Learning Journal policy defined by EDR-007.
7. Microphone recording begins only after a visible user action, shows an active indicator, and stops on user action or the configured silence timeout.
8. The application must clearly display when local content storage is enabled.
9. Diagnostic bundles are generated locally, redacted where possible, previewed by the user, and never transmitted automatically.
10. Destructive data deletion requires a protected confirmation flow.
11. The UI must remind users that local processing does not override employer data-handling policy.

Any future capability requiring external network access requires a new EDR and explicit user approval.

Local observability is permitted: technical metrics may be collected and stored on the workstation under EDR-004 and EDR-005. The prohibition above applies to transmitting telemetry outside the workstation.

## Consequences

- Sensitive data remains under local user control.
- Features depending on cloud services are unavailable.
- Fonts, libraries, models, and other runtime dependencies must be installed or bundled locally.
- Privacy validation becomes an acceptance criterion for relevant phases.

## Alternatives considered

- Cloud LLM or speech APIs: rejected because they violate the required privacy boundary.
- Silent content collection for learning: rejected because content storage requires informed opt-in.
- Automatic diagnostic upload: rejected because the user must inspect and control exported information.

## Compliance

Relevant phase reports must document network binding, external-request checks, content-storage settings, microphone behavior, temporary-file cleanup, and diagnostic export behavior.

## Related records

- EDR-003: System architecture
- EDR-004: Data storage and retention
- EDR-005: Model observability and evaluation
- EDR-007: Learning capture and grammar analysis
