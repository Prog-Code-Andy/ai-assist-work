# Planning Stage 03: Roadmap, Phase, and Prompt Package

## Outcome

The EDR set and Report Output Contract v1.0 were accepted. A proposed end-to-end roadmap, nine proposed implementation phases, one reusable Claude Code master prompt, nine matching numbered phase prompts, a workstation runbook, and synchronized status structure were created.

## Phase sequence

```text
00 Audit and Protection
01 SQLite, Settings, and Privacy
02 Observability, Logs, and Evaluation
03 Learning, Grammar, and Sentence Review
04 Local Voice and macOS
05 Vocabulary
06 Study Materials, Reports, and Export
07 PWA and macOS Packaging
08 Search and Dataset Readiness
```

## Gate status

- EDR-001 through EDR-008: ACCEPTED
- Report Output Contract v1.0: ACCEPTED
- Product Roadmap: PROPOSED FOR REVIEW
- Phase specifications: PROPOSED FOR REVIEW
- Claude Code prompts: PROPOSED FOR REVIEW
- Application phases approved: NONE
- Application code created in this repository: NO

## How execution will work

The user provides `prompts/MASTER.md` to Claude Code on the workstation. The master prompt reads the local configuration and status, loads all authoritative documents, validates an exact `Approve Phase NN` instruction, selects one matching phase prompt, and stops after tests, a completion report, learning documentation, and status updates.

## Review checklist

- Confirm the nine-phase order and scope boundaries.
- Confirm the acceptance criteria and test policy.
- Confirm reports and learning notes remain in the control repository without sensitive content.
- Confirm the two-repository workstation workflow.
- Confirm the single reusable master prompt behavior.
- Confirm Phase 00 remains blocked until separate approval.

## Next gate

After reviewing this package, explicitly approve the roadmap, phases, and prompt package. Only then may project state advance to awaiting `Approve Phase 00`.
