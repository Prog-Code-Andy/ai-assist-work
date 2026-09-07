# Workstation Prompt 000: Validate Repositories and Project State

## Authorization

- Application changes authorized: No
- Phase approval included: No
- Expected result: Read-only readiness report

## Prompt to copy into Claude Code

```text
Read and follow the master prompt completely from the control repository:

prompts/MASTER.md

Use `.project/workstation-config.local.json` to resolve the absolute control repository and application repository paths.

Validate all of the following with read-only checks:

1. The control repository path exists and contains the approved EDRs, Report Output Contract v1.0, Product Roadmap v1.0, phase specifications, and prompt package.
2. The application repository path exists and is separate from the control repository.
3. The local workstation configuration is valid and is ignored by Git.
4. The control project state is valid and identifies Phase 00 as awaiting approval.
5. Both repositories' current branches and working-tree states are reported without changing or discarding any files.
6. Claude Code can read both repositories.

Do not edit either repository.
Do not start Phase 00.
Do not treat this validation request as phase approval.

Return a concise readiness report containing:

- control repository: ready or blocker;
- application repository: ready or blocker;
- project state: ready or blocker;
- Git working trees: clean or describe existing changes;
- exact blocker, if any;
- whether the project is ready to receive the separate command `Approve Phase 00`.
```
