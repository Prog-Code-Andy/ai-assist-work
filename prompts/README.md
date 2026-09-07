# Claude Code Prompts

Use [MASTER.md](MASTER.md) as the single reusable entry prompt in Claude Code. It validates project state and selects one matching numbered prompt from `prompts/phases/`.

Copy/paste prompts used during workstation sessions are stored sequentially in [`prompts/workstation/`](workstation/README.md). This keeps operational conversation prompts separate from the master orchestration rules and phase specifications.

## Naming and execution rules

- Phase prompt filenames begin with the matching phase number: `00-...md`, `01-...md`, `02-...md`.
- Prompts must be executed in numeric order.
- A prompt may run only when its phase is explicitly approved in `.project/phase-state.json`.
- Completion of one prompt never approves the next prompt.
- On completion, Claude must create the required report and learning notes, update both status files, and stop.

The prompt package was approved on 2026-09-07. No application phase is currently approved; Phase 00 requires the exact phrase `Approve Phase 00`.
