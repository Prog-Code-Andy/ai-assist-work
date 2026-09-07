# Phase Prompt 08: Search and Future Dataset Readiness

Execute this prompt only through `prompts/MASTER.md` after Phase 07 is complete and `Approve Phase 08` is validated.

## Required specification

Read and implement only `docs/phases/08-search-and-dataset-readiness.md`.

## Execution instructions

1. Implement SQLite full-text search over eligible learning data and preserve save/delete/index consistency.
2. Evaluate local embeddings only after FTS and implement semantic search only when evidence and approved dependencies justify it.
3. Add explicit future-training approval and revocation controls.
4. Export only approved, privacy-reviewed records to versioned JSONL with provenance and preview.
5. Compare prompt improvement, preferred examples, retrieval, and possible LoRA/fine-tuning; do not train or modify a model.
6. Run the final safe regression and end-to-end local-only validation.
7. Produce the phase report, fine-tuning feasibility assessment, and English learning documentation.
8. Update state to `awaiting_final_acceptance` and stop. Any future implementation requires a new decision and approval.
