# Phase 08: Search and Future Dataset Readiness

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/08-search-and-dataset-readiness.md`
- Dependencies: Completed Phase 07 and explicit Phase 08 approval
- Governing EDRs: EDR-001, EDR-002, EDR-003, EDR-004, EDR-005, EDR-006, EDR-007, EDR-008

## Objective

Make approved historical learning data searchable and assess future model-improvement options without performing fine-tuning.

## Approved scope

- Add SQLite full-text search for eligible sentences, rules, examples, and vocabulary.
- Respect deletion, Journal eligibility, and data-class boundaries in indexing.
- Provide filters and links from search results to the originating learning record.
- Evaluate optional local embeddings with the already available or explicitly approved local embedding model.
- Implement semantic similarity only if the evaluation demonstrates a clear benefit, local resource cost is acceptable, and no new unapproved dependency or network access is required.
- Add explicit per-record and batch `Approve for future training dataset` controls with review and revocation.
- Export only approved, privacy-reviewed records to versioned JSONL with provenance, prompt/model versions, original input, preferred output, feedback, and redaction state.
- Assess prompt improvement, preferred examples, local retrieval, and LoRA/fine-tuning in that order.
- Produce a fine-tuning feasibility report; do not train or modify a model.
- Run the final safe regression suite and end-to-end privacy validation defined by the test inventory.

## Out of scope

- Fine-tuning, LoRA training, model modification, or model publication.
- Automatic approval of training examples.
- Indexing unsaved, deleted, or ineligible content.
- Remote embeddings or vector database.
- Automatic external dataset upload.

## Deliverables

- SQLite FTS search and index lifecycle.
- Optional evidence-backed local semantic search if justified.
- Training-eligibility review state and revocation.
- Approved versioned JSONL export.
- Model-improvement comparison and fine-tuning feasibility report.
- Final regression/privacy evidence.
- Phase report and search/dataset learning notes.

## Acceptance criteria

- Search returns only eligible, non-deleted records and preserves source links.
- Indexes update after save, edit, removal, and protected deletion.
- FTS behavior is useful before semantic search is considered.
- Embedding evaluation clearly records quality, latency, storage, and memory cost; unavailable or unjustified semantic search remains deferred.
- Dataset export contains only explicitly approved records.
- Revoked records do not appear in a newly generated dataset.
- Dataset fields preserve provenance and version information.
- Export preview and redaction review occur before file creation.
- No model training is performed.
- Final tests and privacy checks pass or blockers are explicitly reported.

## Targeted test plan

- FTS indexing, ranking, filters, deletion, and migration tests.
- Search permission and eligibility tests.
- Optional embedding determinism/integration tests when implemented.
- Training approval/revocation matrix.
- JSONL schema, provenance, preview, and redaction tests.
- No-unapproved-record export tests.
- Safe full regression suite and end-to-end local-only checks.

## Manual and privacy validation

- Search sanitized historical mistakes and vocabulary.
- Review a proposed dataset, revoke an item, regenerate, and verify exclusion.
- Verify no deleted/unsaved content is searchable or exported.
- Inspect final application network behavior and local storage boundaries.

## Learning deliverables

- Explain FTS, indexing, ranking, embeddings, semantic search, retrieval, dataset curation, provenance, redaction, LoRA, and fine-tuning tradeoffs in English.

## Completion report

Create `reports/phases/08-completion.md` plus a separate fine-tuning feasibility assessment. Include final regression and privacy evidence, deferred decisions, and recommended future EDRs.

## Rollback guidance

Document rebuilding/removing derived indexes and embeddings without deleting source learning records, and revoking/removing generated dataset files safely.

## Approval gate

Set Phase 08 to complete and project delivery status to `awaiting_final_acceptance`. Stop. Any model training or future feature requires a new explicit decision and user approval.
