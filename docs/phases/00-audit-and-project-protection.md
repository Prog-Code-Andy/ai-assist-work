# Phase 00: Audit and Project Protection

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/00-audit-and-project-protection.md`
- Dependencies: Accepted EDR-001 through EDR-008; approved roadmap and phase package
- Governing EDRs: All

## Objective

Establish a factual, recoverable baseline of the workstation application before changing application behavior.

## Preconditions

- The user has explicitly written `Approve Phase 00`.
- Control and application repository paths are resolved and distinct.
- The application repository is accessible.
- Accepted EDRs and Report Output Contract v1.0 are readable.

## Approved scope

- Inventory the application repository, current architecture, entry points, dependencies, configuration, tests, and documentation.
- Record git state, current branch, remotes, uncommitted changes, and recoverability without discarding user work.
- Identify the actual frontend, backend, Ollama, persistence, logging, microphone, and packaging implementation already present.
- Inspect environment versions and locally installed Ollama models using read-only checks.
- Map existing local and external network references in source and configuration.
- Inventory tests before running them and classify any tests that could access external services, modify data, or take excessive time.
- Run safe baseline tests. A full baseline suite is permitted only after unsafe/external tests are excluded or the user approves them.
- Capture a small, repeatable baseline for an existing writing request when Ollama is available.
- Document risks, gaps, existing behavior, and recommended Phase 01 integration points.
- Create the Phase 00 completion report and learning notes.

## Out of scope

- New application features.
- Refactoring only for style.
- Dependency upgrades.
- Installing or removing Ollama models.
- Destructive git cleanup or modification of existing user changes.
- Enabling external services.

## Deliverables

- Architecture and repository inventory.
- Dependency and runtime inventory.
- Existing-data and migration-risk inventory.
- Network/privacy baseline.
- Test inventory and safe baseline results.
- Performance baseline when locally available.
- Gap map from current application to accepted EDRs.
- Phase 00 report and English learning note.

## Acceptance criteria

- Existing user changes are identified and preserved.
- The application start, stop, and health paths are documented as observed or explicitly marked unknown.
- Every detected external URL or network integration is classified.
- Existing content persistence and logs are documented.
- Test commands and their risk classification are recorded.
- Baseline evidence is reproducible or the exact blocker is documented.
- No feature or dependency change has been made.

## Targeted test plan

- Repository and configuration inspection.
- Safe existing unit tests.
- Existing health check if available.
- One sanitized local Ollama request if already supported.
- No browser, microphone, model-download, or external-network test without explicit need and permission.

## Manual and privacy validation

- Confirm the application is no less recoverable than before the audit.
- Confirm no secrets or message content are copied into the control repository.
- Confirm audit commands did not send prompts externally.

## Learning deliverables

- `docs/learning/phase-notes/00-audit-and-protection.md` in the application repository or configured learning-document location.
- Explain repository layout, git state, application flow, local APIs, test classification, and baseline metrics in English.

## Completion report

Create `reports/phases/00-completion.md` with evidence, commands, results, risks, and recommended Phase 01 entry points.

## Rollback guidance

No application feature changes are expected. Any audit-generated temporary file must be listed and safely removable. Never reset or discard pre-existing changes.

## Approval gate

Set Phase 00 to complete and the project state to `awaiting_approval`. Stop. Do not begin Phase 01 until the user explicitly writes `Approve Phase 01`.
