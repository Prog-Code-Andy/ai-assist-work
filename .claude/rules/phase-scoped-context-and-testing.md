# Phase-Scoped Context and Testing

This rule applies to every AI Assist Work implementation session. It supplements, but never overrides, the Accepted EDRs, contracts, approved roadmap, current phase specification, `CLAUDE.md`, and `prompts/MASTER.md`.

## Work on exactly one phase

- Validate the current phase and its explicit approval before modifying application files.
- Do not inspect, implement, prepare, or test future-phase code beyond what is necessary to preserve compatibility with the current phase.
- Treat the Phase 00 inventory and each accepted completion report as the baseline for later phases.

## Keep repository inspection incremental

- Phase 00 may perform the initial repository, dependency, configuration, network, persistence, logging, and test inventory required by its specification.
- After Phase 00, do not repeat a full recursive repository audit when an approved inventory and completion reports are available.
- At the start of a later phase, read the authoritative control documents required by `prompts/MASTER.md`, the current phase files, the previous completion report, and only the relevant application working set.
- Build the application working set from:
  - files changed or added for the current phase;
  - their direct imports, callers, configuration, schemas, migrations, and interfaces;
  - directly related tests and fixtures;
  - files changed since the previous approved phase report.
- Use focused searches and Git diffs to locate relevant files. Do not print or reread entire directories, generated artifacts, dependency trees, databases, large logs, or unchanged files without a concrete reason.
- Expand the working set only when a dependency, failing test, safety concern, or acceptance criterion provides evidence that it is necessary. Record the reason in the phase report.
- A full repository re-audit requires an explicit phase requirement, evidence that the baseline is stale or incomplete, or separate user approval.

## Run proportionate tests

- Before running tests, use the Phase 00 test inventory to identify unsafe, destructive, external-network, microphone, model-download, or unexpectedly expensive commands.
- Run tests for changed modules, direct dependents, the current phase's required integration paths, and one short relevant smoke path.
- Do not rerun unrelated completed-phase test groups merely because they exist.
- Do not run the full regression suite unless the current phase explicitly requires it or the user separately approves it.
- Phase 08 owns the final safe full regression suite and end-to-end local-only validation defined by the approved test inventory.
- When a targeted test exposes a cross-phase regression, run the smallest additional test set that can confirm and diagnose it. Document why the scope expanded.
- Summarize test output. Retain detailed failure evidence locally and avoid copying sensitive or excessively large output into Claude's context.

## Treat caches correctly

- Do not clear safe test-runner, package-manager, compiler, or build caches by default.
- A cache may improve execution speed, but a previously cached result is not evidence that the current change passed. Required targeted tests must actually execute.
- Clear or bypass a tool cache only when there is evidence of stale or invalid results; state the reason before doing so and record it in the phase report.
- Do not enable application response caching. It is a separate product decision governed by EDR-005 and remains disabled unless a later Accepted EDR changes that decision.
- Never report an Ollama, KV-context, operating-system, or application cache hit unless the runtime exposes reliable evidence for that cache category.

## Completion evidence

- In every phase completion report, list the inspected application working set, test commands executed, tests intentionally not run, any scope expansion, cache invalidation performed, and known limitations.
- Stop at the phase gate after the report, learning documentation, and status updates. Completion never approves the next phase.
