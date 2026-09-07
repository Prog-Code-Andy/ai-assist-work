# Workstation Prompt 001: Run Master Read-Only Validation

## Authorization

- Application changes authorized: No
- Control repository changes authorized: No
- Phase approval included: No
- Expected result: Read-only readiness report and blockers

## When to use

Use this prompt when Claude Code has opened or reviewed an individual phase prompt directly instead of starting through `prompts/MASTER.md`.

## Prompt to copy into Claude Code

```text
Do not start Phase 00.

Locate the control repository using the open VS Code workspace and its local configuration file:

.project/workstation-config.local.json

Read and follow this file completely from that control repository:

prompts/MASTER.md

Then perform the read-only validation defined in:

prompts/workstation/000-validate-control-and-application-repositories.md

Use `.project/workstation-config.local.json` to resolve both repository paths.

Do not modify either repository.
Do not treat this message as approval for Phase 00.
Do not execute any phase implementation prompt.

Return only the readiness report and any blockers.
```

## Expected stop condition

Claude Code returns the readiness report and stops. The user reviews that report before issuing any separate phase approval.
