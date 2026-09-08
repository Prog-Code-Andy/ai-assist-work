# Workstation Copy/Paste Prompts

This directory is the chronological library of user-facing prompts copied from GitHub into Claude Code during the AI Assist Work implementation.

## Purpose

- Preserve every approved workstation instruction used during the project.
- Let the user copy one complete prompt without reconstructing it from chat history.
- Keep session-level instructions separate from `MASTER.md` and the authoritative phase prompts.
- Provide a reviewable history on the long-lived `workstream/project-prompts` branch.

## Naming

Use a three-digit sequence and descriptive name:

```text
000-validate-control-and-application-repositories.md
001-run-master-read-only-validation.md
002-audit-phase-01-test-scope.md
003-inspect-stuck-browser-transcription.md
004-review-observability-and-voice-remediation.md
005-implement-observability-and-voice-remediation.md
006-next-approved-workstation-action.md
```

Numbers describe the order in which prompts should be used. Never reuse or renumber a published prompt. A correction receives the next number and links to the prompt it replaces.

## Rules

1. Store only reusable instructions, never Claude responses or sensitive work content.
2. Do not include workstation usernames, secrets, corporate messages, or machine-specific absolute paths.
3. Reference `.project/workstation-config.local.json` for local paths.
4. Every prompt states whether it can modify the application and whether phase approval is included.
5. A workstation prompt cannot override Accepted EDRs, contracts, the approved roadmap, or phase gates.
6. Adding a prompt to this library does not approve a phase.
7. Keep this workstream branch unmerged until the user explicitly approves the final merge.

## Prompt index

| Sequence | Prompt | Purpose | Application changes authorized |
|---:|---|---|---|
| 000 | [Validate repositories and state](000-validate-control-and-application-repositories.md) | Confirm workstation readiness before Phase 00 | No |
| 001 | [Run master read-only validation](001-run-master-read-only-validation.md) | Correct the entry sequence when an individual phase prompt was opened directly | No |
| 002 | [Audit Phase 01 test scope](002-audit-phase-01-test-scope.md) | Verify targeted-test compliance without rerunning tests or starting Phase 02 | No |
| 003 | [Inspect stuck browser transcription](003-inspect-stuck-browser-transcription.md) | Trace the microphone-to-transcript path without editing files or running tests | No |
| 004 | [Review observability and voice remediation](004-review-observability-and-voice-remediation.md) | Validate EDR-009 and REM-001 against the workstation without changes | No |
| 005 | [Implement observability and voice remediation](005-implement-observability-and-voice-remediation.md) | Execute accepted REM-001 only after its exact approval gate | Conditional |
| 006 | [Verify repositories and select the next approved action](006-next-approved-workstation-action.md) | Distinguish the control package from the Local Corporate Writing Assistant application and reconcile workstation state | No |
