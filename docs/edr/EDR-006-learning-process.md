# EDR-006: Required Learning Process and Documentation

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

The user wants to understand the system rather than receive only generated code. The project includes unfamiliar areas such as macOS microphone permissions, local speech processing, FastAPI, Ollama integration, SQLite, observability, caching, packaging, and troubleshooting. Knowledge must survive individual Claude sessions.

## Decision

Learning documentation is a mandatory deliverable of every implementation phase.

Each phase must explain in clear English:

- what was built;
- why the architecture and tools were selected;
- which files and components were created or changed;
- how components communicate and where privacy boundaries exist;
- which commands or tools were used and what they do;
- how relevant macOS capabilities and permissions work;
- how the user can locate and inspect the relevant code;
- how to validate the result and what output to expect;
- which problems occurred, what evidence was collected, how they were diagnosed, and how they were resolved;
- known limitations and topics for further study.

Documentation is organized under:

```text
docs/learning/
├── GLOSSARY.md
├── architecture/
├── tools/
├── integrations/
├── macos/
├── troubleshooting/
└── phase-notes/
```

At each completion gate Claude offers a learning checkpoint:

1. Ask whether the user understands the implementation.
2. Offer a deeper explanation of selected components.
3. Offer to inspect the relevant code together.
4. Allow the user to mark a topic understood or skip the checkpoint.

Skipping a checkpoint does not remove the required written learning documentation. Learning questions do not approve the next implementation phase.

`docs/learning/GLOSSARY.md` is a continuously maintained junior-friendly vocabulary. When a project document introduces an abbreviation or unfamiliar technical term, Claude must add a concise explanation, its purpose, how it relates to AI Assist Work, and a small example when useful.

## Consequences

- The repository becomes a persistent learning resource rather than only an execution script collection.
- Troubleshooting knowledge remains available after the immediate problem is fixed.
- Every phase requires additional documentation effort.
- Explanations must link to real implementation evidence and avoid generic tutorials unrelated to delivered work.

## Alternatives considered

- Completion reports only: rejected because operational reporting does not adequately teach architecture, tools, and troubleshooting.
- Verbal explanations only: rejected because knowledge would be lost between sessions.
- Optional learning notes: rejected because documentation would become inconsistent in difficult phases.

## Compliance

A phase cannot be marked complete until its required learning note exists and the completion report links to it. Phase review must verify that explanations match the implementation.

## Related records

- EDR-001: Phase-gated execution governance
- EDR-003: System architecture
- EDR-004: Data storage and retention
- EDR-005: Model observability and evaluation
- EDR-007: Learning capture and grammar analysis
- EDR-008: Study materials, reporting, and export
