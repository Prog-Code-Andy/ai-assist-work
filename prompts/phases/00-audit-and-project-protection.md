# Phase Prompt 00: Audit and Project Protection

Execute this prompt only through `prompts/MASTER.md` after validated `Approve Phase 00` authorization.

## Required specification

Read and implement only `docs/phases/00-audit-and-project-protection.md`.

## Execution instructions

1. Inspect both repositories and preserve all existing user work.
2. Audit application structure, runtime, dependencies, configuration, tests, network references, persistence, logs, Ollama integration, voice, and packaging as they actually exist.
3. Classify tests before execution. Do not run unsafe, external, destructive, or unexpectedly expensive tests without approval.
4. Establish safe functional, privacy, and performance baselines where available.
5. Do not add features, upgrade dependencies, install/remove models, or refactor application code.
6. Produce the required audit report and English learning note.
7. Update state, request `Approve Phase 01`, and stop.

## Completion evidence

The report must distinguish observed facts, inferences, unavailable checks, risks, and recommended Phase 01 integration points.
