# Workstation Prompt 002: Audit Phase 01 Test Scope

## Authorization

- Application changes authorized: No
- Control repository changes authorized: No
- Test execution authorized: No
- Phase approval included: No
- Expected result: Evidence-based Phase 01 test-scope audit

## When to use

Use this prompt after Phase 01 reports completion when its test summary includes pre-existing tests and the user needs to verify compliance with the phase-scoped testing rule before approving Phase 02.

## Prompt to copy into Claude Code

```text
Do not start Phase 02.
Do not modify either repository.
Do not rerun any tests.

Use `.project/workstation-config.local.json` from the control repository to resolve both repository paths.

Confirm whether the following application-repository rule was loaded into Claude Code context while Phase 01 was executed:

.claude/rules/phase-scoped-context-and-testing.md

Do not treat the file's presence on disk or in Git as proof that it was loaded. Use available session/context evidence. If loading cannot be proven, report it as `not verified` and instruct the user to run `/context` manually.

Audit the Phase 01 test scope using only existing evidence, including the Phase 01 completion report, command history when safely available, test files, Git diff, and recorded results. Do not execute the tests again.

Return only this concise audit:

1. Rule loading: `verified`, `not loaded`, or `not verified`, with evidence.
2. Exact test commands previously executed.
3. The 29 new tests, grouped by affected Phase 01 module or behavior.
4. The 26 pre-existing tests, grouped by their direct relationship to Phase 01.
5. Any pre-existing tests unrelated to Phase 01.
6. Whether the complete test suite was executed.
7. The recorded reason for expanding beyond changed modules, direct dependents, required Phase 01 integration tests, and one Writer smoke path.
8. Whether the Phase 01 completion report records:
   - inspected application working set;
   - executed test commands;
   - tests intentionally not run;
   - test-scope expansion and its reason;
   - cache invalidation, or an explicit statement that none occurred.
9. Compliance conclusion: `compliant`, `partially compliant`, `non-compliant`, or `insufficient evidence`.
10. The smallest documentation-only correction required before Phase 02 approval, if any.

Do not infer that a test was relevant merely because it passed. Base the relationship on the changed code, imports, callers, interfaces, persistence behavior, or the approved Phase 01 test plan.

Stop after returning the audit. This message does not approve Phase 02.
```

## Expected stop condition

Claude Code returns the audit without editing files or running tests. The user reviews the evidence before deciding whether Phase 01 documentation needs correction and whether Phase 02 may be approved.
