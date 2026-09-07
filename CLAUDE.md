# Claude Code Governance for AI Assist Work

This repository is the documentation and execution-control repository. It is not the application source repository.

## Authority order

1. Explicit current user instruction and approval boundary.
2. Accepted EDRs in `docs/edr/`.
3. Accepted versioned contracts in `docs/contracts/`.
4. Approved Product Roadmap.
5. Approved phase specification.
6. Matching numbered phase prompt.
7. Existing application conventions that do not conflict with the above.

If two sources conflict, stop and report the conflict. Never silently choose the more permissive interpretation.

## Immutable records

Never edit an Accepted EDR or Accepted contract. A material change requires a new proposed EDR and, when applicable, a new contract version.

## Repository boundary

- Control repository: EDRs, contracts, roadmap, phases, prompts, status, reports, and learning documentation.
- Application repository: application code, application tests, migrations, assets, and runtime configuration examples.
- Resolve both absolute paths through `.project/workstation-config.local.json`.
- Never place application source code in the control repository.
- Never copy secrets, real work messages, raw audio, or sensitive database content into the control repository.

## Phase gate

- Use `prompts/MASTER.md` as the only reusable entry point.
- Execute only the phase that is explicitly approved and recorded in `.project/phase-state.json`.
- The exact approval phrase is `Approve Phase NN`.
- The user does not manually edit status files. Claude updates them after validating the explicit approval.
- Completion of one phase never approves the next.
- After tests, report, learning documentation, and status updates, stop and request approval.
- Never continue because the next phase appears obvious, small, or related.

## Safety and privacy

- Keep runtime communication local to the workstation and loopback.
- Do not add external AI, speech, analytics, telemetry, database, CDN, font, or upload services.
- Do not install/remove models or perform destructive actions without explicit user authorization.
- Preserve existing user changes and dirty worktrees.
- Do not put message bodies in diagnostics by default.

## Testing

- Run tests for changed modules, direct dependents, and one short smoke path.
- Do not repeatedly run the entire suite.
- Run a full suite only where a phase explicitly defines it and only after unsafe/external tests are identified, or after explicit user approval.
- Summarize console output; retain detailed local failure evidence without copying sensitive data.

## Learning and reporting

- Every completed phase requires an English learning note and a completion report.
- Explain real delivered architecture, tools, commands, macOS behavior, validation, and troubleshooting.
- Update `docs/learning/GLOSSARY.md` when introducing unfamiliar terms.
- Offer one meaningful learning checkpoint after the phase.
- Learning checkpoint responses do not approve the next phase.
