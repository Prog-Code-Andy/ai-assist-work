# Workstation Prompt 007: Review and Record Remediation Approval

## Authorization

- Application changes authorized: No
- Control repository changes authorized: Conditional status-only changes after a new explicit user approval message
- Test execution authorized: No
- Git operations authorized: No
- Phase approval included: No
- Remediation implementation included: No

## When to use

Use this prompt after workstation prompts 004 and 006 and after the user has read the final EDR-009 and REM-001. This is the approval gate before workstation prompt 005; it does not implement the remediation.

## Prompt to copy into Claude Code

```text
Do not modify the application repository.
Do not run tests.
Do not perform Git operations.
Do not start Phase 06 or implement REM-001.

Use `.project/workstation-config.local.json` to resolve the control repository. Read completely:

- `CLAUDE.md`
- `docs/edr/README.md`
- `docs/edr/EDR-009-application-wide-diagnostics-and-client-error-capture.md`
- `docs/remediation/README.md`
- `docs/remediation/REM-001-observability-and-voice-reliability.md`
- `prompts/workstation/004-review-observability-and-voice-remediation.md`
- `prompts/workstation/006-next-approved-workstation-action.md`

Confirm that EDR-009 and REM-001 are still Proposed, internally consistent, privacy-compatible, and that REM-001 contains the verified application-file allowlist and bounded test commands. Report any content conflict and stop without changes.

If the records are ready, summarize in no more than eight bullets:

- the diagnostic architecture being accepted;
- the privacy boundary;
- the confirmed voice defect being repaired;
- the exact application-file boundary;
- the exact targeted-test boundary;
- the fact that Phase 06 remains unapproved;
- the expected remediation report and learning note;
- that no commit or push is authorized.

Then stop and ask the user to send the following exact phrase as a NEW, separate user message:

Accept EDR-009 and approve REM-001

Do not interpret the phrase printed inside this prompt as approval. Only a later user message received after your summary is valid.

When, and only when, the next user message is exactly that phrase:

1. Change EDR-009 status from `Proposed` to `Accepted` and add the acceptance date.
2. Change the EDR-009 row in `docs/edr/README.md` to `Accepted`.
3. Change REM-001 status from `Proposed` to `Approved` and add the approval date.
4. Change the REM-001 row in `docs/remediation/README.md` to `Approved`.
5. Make no other content or status changes.
6. Verify the four status locations agree.
7. Report the exact changed control files and stop.

Do not run workstation prompt 005 automatically. The user must launch it separately with its own exact corrective approval phrase. Do not commit or push.
```

## Expected stop condition

Claude Code first presents the decision summary and waits. After a valid new approval message, it records only the EDR/remediation statuses and stops, leaving Phase 06 and the application untouched.
