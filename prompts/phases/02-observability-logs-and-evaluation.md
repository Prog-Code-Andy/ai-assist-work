# Phase Prompt 02: Observability, Logs, and Model Evaluation

Execute this prompt only through `prompts/MASTER.md` after Phase 01 is complete and `Approve Phase 02` is validated.

## Required specification

Read and implement only `docs/phases/02-observability-logs-and-evaluation.md`.

## Execution instructions

1. Implement structured JSONL diagnostics and SQLite model/performance records with explicit provenance and units.
2. Capture available Ollama metrics and application-observed streaming timings without inventing unavailable values.
3. Sample aggregate workstation and AI Assist Work/Ollama resource use only around active requests.
4. Do not monitor unrelated process identities by default and never control unrelated processes.
5. Keep message bodies out of diagnostics and keep response caching disabled.
6. Implement bounded, redacted, previewable local diagnostic bundles with no automatic transmission.
7. Measure observability overhead and validate rotation, retention, cancellation, and error behavior.
8. Produce the report and English learning documentation, update state, request `Approve Phase 03`, and stop.
