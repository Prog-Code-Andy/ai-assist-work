# Learning Note: Planning and Execution Control Package

## What was built

The control repository now contains accepted engineering decisions, an output contract, a proposed roadmap, proposed implementation phases, ordered phase prompts, a reusable master prompt, machine/human status files, and a workstation runbook.

## Why this approach was selected

The product is too broad and privacy-sensitive for one uncontrolled implementation run. The control package separates durable decisions from roadmap goals, executable phase scope, and completion evidence.

## Core relationships

```text
EDR → Roadmap → Phase specification → Phase prompt
    → Application changes → Tests → Report → Learning note → Approval
```

## Important concepts

- EDR records why an engineering rule exists.
- A contract fixes an exact output structure.
- The roadmap orders outcomes without acting as an implementation command.
- A phase specification defines boundaries and acceptance criteria.
- The master prompt reads state and selects only one approved phase prompt.
- JSON is the machine-readable source of state; Markdown is the human-readable view.

## How to validate

Read `docs/edr/README.md`, `docs/contracts/README.md`, `docs/roadmap/PRODUCT-ROADMAP.md`, `docs/phases/README.md`, `prompts/MASTER.md`, and `.project/phase-state.json`. Confirm that no application phase is approved before package review.

## Learning checkpoint

- Can you explain why completion of one phase does not approve the next?
- Can you explain the difference between an EDR, roadmap, phase specification, and prompt?
- Can you identify which repository stores application code and which stores governance?
