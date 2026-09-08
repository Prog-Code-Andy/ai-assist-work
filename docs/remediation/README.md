# Corrective Work Packages

Corrective work packages address confirmed defects or accepted-requirement gaps without rewriting completed phase history or automatically completing a future phase.

## Rules

- A package must identify its governing EDRs and the phase requirements it repairs.
- A package runs only after its proposed EDRs are Accepted and the user gives the package's exact approval phrase.
- Corrective work does not approve, complete, or advance an implementation phase unless the package explicitly says so.
- Testing remains targeted. A full regression suite requires a separate approved reason.
- Completion requires a remediation report, learning note, status evidence, and a stop for user review.

## Current packages

| Package | Purpose | Status |
|---|---|---|
| [REM-001](REM-001-observability-and-voice-reliability.md) | Close application-log coverage gaps and repair the confirmed voice-recorder stop race | Proposed |

