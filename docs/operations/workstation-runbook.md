# AI Assist Work Workstation Runbook

## Purpose

Use this runbook to apply the planning package from GitHub to the separate application repository with Claude Code.

## Repository layout

Keep the repositories separate and accessible from the same workstation account:

```text
workspace/
├── ai-assist-work/              control repository
└── ai-assist-work-application/  application repository
```

The names may differ. Record their actual absolute paths in the ignored local configuration.

## 1. Obtain the control repository

Create the GitHub repository and push this control package, or clone/pull it on the workstation using the organization's approved Git workflow.

Do not commit:

- `.project/workstation-config.local.json`;
- `.env` files;
- real prompts or corporate messages;
- SQLite application data;
- raw audio;
- diagnostic bundles containing workstation information.

## 2. Prepare local path configuration

Copy `.project/workstation-config.example.json` to:

```text
.project/workstation-config.local.json
```

Replace the example values with the actual absolute paths of the control and application repositories. The local file is ignored by Git.

If this is not done manually, the master prompt will ask for the two paths and create the ignored file after verifying them.

## 3. Start Claude Code

Open Claude Code in a directory where it can read the control repository and work in the application repository. Give it the complete contents of:

```text
prompts/MASTER.md
```

Use the same master prompt after interruptions or between phases. Do not manually choose a numbered phase prompt; the master prompt reads project state and selects it.

## 4. Approve one phase

After the entire planning package is reviewed and approved, the first execution command is:

```text
Approve Phase 00
```

Claude validates the phrase, updates status, and executes only Phase 00.

After completion, review:

- the application behavior or audit result;
- `reports/phases/NN-completion.md`;
- the matching English learning note;
- tests and limitations;
- `.project/PHASE-STATUS.md`.

Ask questions or inspect code with Claude. Learning discussion does not approve the next phase.

When satisfied, use the exact next phrase, for example:

```text
Approve Phase 01
```

The user never needs to check boxes or edit JSON manually. Claude updates both status files after explicit approval and completion.

## 5. Sync through GitHub

After reviewing a completed phase:

1. Inspect changes in both repositories.
2. Confirm reports and learning notes contain no sensitive work content.
3. Commit through the approved workflow.
4. Push the control documentation and application changes to their appropriate repositories.
5. Pull the latest approved state before resuming on another machine.

Never resolve conflicting phase-state files by guessing. Compare completion reports and git history, correct the state with the user, and only then resume.

## Recovery

If Claude stops or the session changes:

1. Open the control and application repositories.
2. Pull or inspect the latest reviewed state.
3. Paste `prompts/MASTER.md` again.
4. Claude reads `.project/phase-state.json` and resumes only the legal approved action.

If a phase is blocked, do not approve the next phase. Resolve the blocker or explicitly revise the plan through the appropriate decision process.

## Final boundary

The master prompt orchestrates the full project over multiple approvals. It never grants itself permission to execute all phases in one run.
