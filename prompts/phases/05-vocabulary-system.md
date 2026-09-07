# Phase Prompt 05: Vocabulary System

Execute this prompt only through `prompts/MASTER.md` after Phase 04 is complete and `Approve Phase 05` is validated.

## Required specification

Read and implement only `docs/phases/05-vocabulary-system.md`.

## Execution instructions

1. Implement canonical vocabulary items, senses, examples, occurrences, collocations, synonyms, provenance, confidence, and learning status.
2. Ensure repeated uses update occurrence history rather than create duplicate canonical words.
3. Preserve separate meanings and context-specific Russian translations under one canonical word.
4. Distinguish user-used vocabulary from model recommendations.
5. Apply configurable stop-word filtering and review uncertain normalization rather than merging it silently.
6. Keep pronunciation local and honor Journal/per-entry persistence controls.
7. Do not implement historical export or semantic search early.
8. Produce the report and English learning documentation, update state, request `Approve Phase 06`, and stop.
