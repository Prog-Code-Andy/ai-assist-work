# AI Assist Work

This repository is the documentation and execution-control package for the AI Assist Work application.

It contains engineering decisions, the product roadmap, implementation phases, ordered Claude Code prompts, completion reports, and learning documentation. Application source code does not belong in this repository.

## Repository workflow

1. Approve and record Engineering Decision Records (EDRs).
2. Build the product roadmap from accepted EDRs.
3. Define implementation phases and acceptance criteria.
4. Create numbered Claude Code prompts for the approved phases.
5. Execute one approved phase at a time on the workstation.
6. Record tests, reports, learning notes, and phase status.
7. Stop and wait for explicit approval before the next phase.

## Structure

```text
docs/edr/              Engineering Decision Records
docs/contracts/        Versioned output and data contracts
docs/roadmap/          Product roadmap
docs/phases/           Detailed implementation phases
docs/learning/         Persistent learning documentation
prompts/               Ordered Claude Code execution prompts
reports/phases/        Phase completion reports
templates/             Reusable document templates
.project/              Machine-readable and human-readable status
```

## Current scope

EDR-001 through EDR-008, Report Output Contract v1.0, Product Roadmap v1.0, implementation phases, and the Claude Code prompt package are approved. Phase 00 is selected but still requires its own explicit approval. No application source code belongs in this repository.

## Start here

1. Review [the product roadmap](docs/roadmap/PRODUCT-ROADMAP.md).
2. Review [the phase index](docs/phases/README.md).
3. Review [the master prompt](prompts/MASTER.md).
4. Follow [the workstation runbook](docs/operations/workstation-runbook.md).
5. Start Phase 00 only after the user explicitly writes `Approve Phase 00`.
