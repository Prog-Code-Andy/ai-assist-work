# EDR-001: Phase-Gated Execution Governance

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

AI Assist Work will be implemented on a separate workstation from ordered Claude Code prompts stored in this repository. Long autonomous runs can accidentally skip work, begin an unapproved phase, rerun expensive validation, or mix the scope of multiple phases.

## Decision

Implementation will use a strict phase gate:

```text
Explicit approval → Implement → Targeted tests → Completion report
→ Learning documentation → Await explicit approval
```

The following controls are mandatory:

1. Every implementation phase and matching prompt has the same sequential number.
2. Prompts execute only in numeric order.
3. `.project/phase-state.json` is the machine-readable source of execution state.
4. `.project/PHASE-STATUS.md` is the human-readable status view.
5. Claude verifies the approved phase, dependencies, and prior report before changing application files.
6. Claude may automatically mark completed work as complete after satisfying its acceptance criteria.
7. Completion never constitutes approval of the next phase.
8. The next phase begins only after explicit user approval.
9. Claude stops after producing the report, learning documentation, and status update.
10. A full test suite requires either an explicit phase requirement or separate user approval. Normal phase validation uses changed and directly related tests plus a short smoke test.

Accepted EDRs are immutable. A changed decision requires a later EDR that explicitly amends or supersedes this record.

## Consequences

- Execution is resumable and auditable across machines and Claude sessions.
- The user does not need to remember which prompt ran last.
- Each phase has a clear recovery point and evidence of completion.
- Additional status-maintenance work is required after every phase.
- Incorrect or inconsistent status must block execution rather than be guessed.

## Alternatives considered

- One master prompt implementing the entire product: rejected because it weakens review and recovery boundaries.
- Markdown checkboxes as the only state: rejected because they are easy to edit inconsistently and are less reliable for automated checks.
- Automatic continuation after successful tests: rejected because success does not equal authorization.

## Compliance

Every phase and prompt must reference this EDR. Completion reports must state that no next-phase implementation began. Status updates must keep JSON and Markdown synchronized.

## Related records

- EDR-002: Local-only privacy boundary
- EDR-005: Model observability and evaluation
- EDR-006: Learning process
