# EDR-005: Model Observability, Logs, and Evaluation

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

AI Assist Work is also a learning environment for understanding local-model behavior. The user needs reliable token, latency, resource, cache, quality, and error information that can support weekly analysis, troubleshooting, prompt improvement, model comparison, and a possible future fine-tuning decision.

## Decision

Model observability is a first-class subsystem rather than incidental application logging.

For every eligible generation, record available technical facts including:

- request and session identifiers;
- timestamp, model name/version, writing mode, prompt version, and generation settings;
- input and output token counts reported by the runtime;
- model load, prompt evaluation, generation, total, and first-token timing where measurable;
- derived prompt and output throughput with calculation version;
- completion, cancellation, timeout, and error status;
- context-window configuration;
- system-wide CPU utilization before, during, and immediately after generation;
- CPU utilization attributable to the local application and Ollama process when safely measurable;
- system memory use, memory pressure, and swap activity during the same observation window;
- thermal, GPU, or accelerator indicators only when macOS exposes them through safe and reliable local interfaces;
- warm/cold classification based on documented evidence;
- application-managed cache events if application caching is later approved;
- user rating, rating reason, and whether the output was edited before use when evaluation storage is enabled.

Observability rules:

1. Store raw technical facts separately from derived metrics.
2. Never invent a cache-hit measurement that Ollama does not expose.
3. Distinguish model residency, KV context cache, application response cache, and operating-system caching.
4. Application response caching is disabled initially because it can distort benchmarks and retain sensitive content.
5. Resource sampling runs only around active generation and must not become background surveillance. Default collection uses aggregate system load and the AI Assist Work/Ollama processes. Recording names or activity of unrelated processes requires a separate explicit diagnostic option because process metadata may be sensitive.
6. JSONL diagnostics rotate locally; queryable metrics live in SQLite.
7. Claude-facing diagnostic bundles are filtered summaries, not unrestricted log dumps.
8. Bundle generation removes duplicates, groups errors, highlights anomalies, applies redaction, and provides a local preview before export.
9. Nothing is automatically sent to Claude or another service.
10. Weekly and monthly reports may compare models, modes, prompt versions, token use, latency, errors, and user evaluations.
11. Reports must correlate response performance with concurrent system load so that a slow request can be distinguished from model, configuration, model-loading, memory-pressure, thermal, or competing-workload effects.
12. Initial behavior is observational: the application records and explains load but does not terminate, suspend, reprioritize, or otherwise control unrelated processes. Any automatic delay or workload-management behavior requires a later decision and explicit user approval.

Future fine-tuning is deferred. Before approval it requires a curated dataset built from explicitly eligible original input, model response, user-edited result, feedback, task mode, prompt/model version, and privacy/redaction status. Prompt improvement, examples, and retrieval must be evaluated before training.

## Consequences

- Model behavior and performance can be studied with reproducible evidence.
- Compact summaries reduce the context required for Claude-assisted analysis.
- Additional database, aggregation, redaction, and UI work is required.
- Some cache behavior may remain unobservable and must be reported as unknown.

## Alternatives considered

- Plain text logs only: rejected because reliable aggregation and machine analysis require structured fields.
- Log full prompts by default: rejected because metrics do not require sensitive message content.
- Enable response caching immediately: rejected because it compromises evaluation and privacy.
- Begin fine-tuning as soon as examples exist: rejected because the dataset first requires quality labels, provenance, and privacy review.

## Compliance

The observability phase must define metric provenance, units, calculation formulas, sampling boundaries, rotation, redaction, retention, and diagnostic-bundle limits. Reports must distinguish measured, derived, and unavailable values.

## Related records

- EDR-002: Local-only privacy boundary
- EDR-003: System architecture
- EDR-004: Data storage and retention
- EDR-006: Learning process
- EDR-007: Learning capture and grammar analysis
