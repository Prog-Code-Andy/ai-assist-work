# Workstation Prompt 006: Verify Repositories and Select the Next Approved Action

## Authorization

- Application changes authorized: No
- Control repository changes authorized: No
- Test execution authorized: No
- Git commit, push, pull, merge, or checkout authorized: No
- Phase or remediation approval included: No
- Expected result: Read-only repository identity, state, and next-action report

## When to use

Use this prompt after workstation prompt 004 and before accepting EDR-009, approving REM-001, running workstation prompt 005, or starting Phase 06.

The two repositories have different purposes:

- `ai-assist-work` is the control repository containing EDRs, phases, prompts, reports, and learning documentation.
- `local-corporate-writing-assistant` is the Local Corporate Writing Assistant application repository containing the application code.

Never assume that the control repository is the application repository.

## Prompt to copy into Claude Code

```text
Do not modify either repository.
Do not run tests.
Do not run git pull, commit, push, merge, checkout, reset, clean, or stash.
Do not approve or start Phase 06.
Do not approve or implement REM-001.

Locate the control repository from the open VS Code workspace. Read completely:

- `CLAUDE.md`
- `prompts/MASTER.md`
- `.project/workstation-config.local.json`
- `.project/phase-state.json`
- `.project/PHASE-STATUS.md`
- `docs/edr/EDR-009-application-wide-diagnostics-and-client-error-capture.md`
- `docs/remediation/REM-001-observability-and-voice-reliability.md`
- `prompts/workstation/004-review-observability-and-voice-remediation.md`
- `prompts/workstation/005-implement-observability-and-voice-remediation.md`

Use only `.project/workstation-config.local.json` to resolve the two local paths. Do not substitute a username or hard-code a workstation path.

Validate that:

1. `control_repository` resolves to the `ai-assist-work` governance repository.
2. `application_repository` resolves to the `local-corporate-writing-assistant` application repository for Local Corporate Writing Assistant.
3. The paths are different directories and both are Git repositories.
4. The control repository contains `prompts/MASTER.md`, `.project/phase-state.json`, and the EDR/remediation documents.
5. The application repository contains the existing application source and its test directory.

For each repository, perform only the read-only equivalents of:

- `git status --short --branch`
- `git branch --show-current`
- `git log -1 --oneline`
- `git remote -v`

Do not print workstation usernames. Redact the home-directory portion of absolute paths as `$HOME` in the report.

Then compare the control repository's machine-readable phase state, human-readable phase status, existing phase completion reports, and the state reported by workstation prompt 004. Do not repair discrepancies.

Return only:

1. A two-row table identifying the control repository and application repository, their redacted paths, branches, HEAD commits, and worktree state.
2. The effective completed phase, current phase, and approval state supported by the local evidence.
3. Every state discrepancy or missing file that must be resolved before REM-001.
4. Whether EDR-009 is Accepted and REM-001 is Approved.
5. Exactly one next action:
   - `synchronize control repository state`,
   - `revise EDR-009 or REM-001`,
   - `request EDR-009 and REM-001 review`, or
   - `ready to run workstation prompt 005 after exact approval`.

Do not treat this prompt as approval. Stop after the report.
```

## Expected stop condition

Claude Code reports which repository is the control package, which is the Local Corporate Writing Assistant application, reconciles the available state evidence without changing it, and identifies exactly one safe next action.
