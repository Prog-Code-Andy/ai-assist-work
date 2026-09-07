# Phase 02: Observability, Logs, and Model Evaluation

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/02-observability-logs-and-evaluation.md`
- Dependencies: Completed Phase 01 and explicit Phase 02 approval
- Governing EDRs: EDR-001, EDR-002, EDR-004, EDR-005

## Objective

Make model and workstation behavior measurable through local, structured, privacy-safe diagnostics and metrics.

## Approved scope

- Add structured JSONL diagnostic events with severity, rotation by size/age, retention, and disk cap.
- Add SQLite performance and evaluation records with explicit units and provenance.
- Capture available Ollama prompt/output token counts, load duration, prompt-evaluation duration, generation duration, total duration, completion reason, and model/configuration identifiers.
- Measure application-observed time to first token and end-to-end latency.
- Sample aggregate system CPU, application/Ollama CPU where safe, memory, memory pressure, and swap only around active generation.
- Record thermal/GPU/accelerator indicators only if safe, local, reliable, and documented during implementation.
- Derive throughput without overwriting raw measurements.
- Distinguish model residency, KV context cache, application cache, and unknown cache state. Do not invent cache hits.
- Keep application response caching disabled.
- Add Application Control pages for Logs and Diagnostics and Performance.
- Add filters and compact on-screen performance summaries.
- Generate a local, redacted, previewable diagnostic bundle with a bounded summary and selected evidence.
- Establish Good/Bad feedback storage foundations without implementing the full learning engine.

## Out of scope

- Continuous background surveillance.
- Recording unrelated process names by default.
- Controlling, terminating, suspending, or reprioritizing other processes.
- Message bodies in diagnostic logs by default.
- External telemetry or automatic diagnostic upload.
- Fine-tuning.

## Deliverables

- Documented diagnostic event schema.
- Rotation, retention, and disk-cap behavior.
- Metrics schema and aggregation services.
- Request-scoped resource sampler.
- Performance and diagnostics settings UI.
- Diagnostic bundle generator, preview, and local export.
- Phase report and English observability learning notes.

## Acceptance criteria

- Every displayed metric identifies its source, unit, and measured/derived/unavailable state.
- A completed streaming request persists accurate runtime totals without duplicating the request.
- Cancelled and failed requests have clear terminal states.
- Resource sampling begins and ends with the request window.
- Diagnostic logs omit message bodies and sensitive headers by default.
- Rotation and retention work under deterministic tests.
- Diagnostic bundles are bounded, redacted, previewed, and never sent automatically.
- High concurrent CPU or memory pressure can be correlated with request latency when samples exist.
- Missing macOS metrics display as unavailable rather than zero.

## Targeted test plan

- Ollama stream-final-metrics parsing with fixtures.
- Timing and throughput formula tests.
- Success, cancellation, timeout, and error lifecycle tests.
- Resource-sampler start/stop and unavailable-metric tests.
- JSONL schema, rotation, retention, and disk-cap tests.
- Redaction, deduplication, bundle-size, and preview tests.
- No-message-body logging tests.
- Existing Writer and Settings smoke tests.

## Manual and privacy validation

- Run sanitized warm and cold local requests and compare captured facts.
- Create controlled competing CPU load only if safe and explicitly permitted, then verify correlation rather than causal overclaiming.
- Inspect logs and diagnostic bundle for user content and secrets.
- Verify no telemetry leaves loopback.

## Learning deliverables

- Explain telemetry versus observability, tokens, timings, throughput, CPU, shared memory, memory pressure, caching categories, JSONL, rotation, and redaction in English.
- Document which macOS measurements are reliable and which remain unavailable.

## Completion report

Create `reports/phases/02-completion.md` with metric provenance, log examples using sanitized data, tests, measured overhead, privacy validation, and limitations.

## Rollback guidance

Document how to disable metrics and logging, stop sampling, retain or remove generated logs safely, and roll back schema changes without deleting unrelated learning data.

## Approval gate

Set Phase 02 to complete and stop at `awaiting_approval`. Do not begin Phase 03 without `Approve Phase 03`.
