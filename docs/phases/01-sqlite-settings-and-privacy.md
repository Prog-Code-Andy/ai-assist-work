# Phase 01: SQLite, Settings, and Privacy Foundation

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/01-sqlite-settings-and-privacy.md`
- Dependencies: Completed Phase 00 and explicit Phase 01 approval
- Governing EDRs: EDR-001, EDR-002, EDR-003, EDR-004, EDR-007

## Objective

Create the local persistence, configuration, consent, retention, and Application Control foundation required by later features while preserving the existing Writer workflow.

## Approved scope

- Add a versioned SQLite migration mechanism and repository/service boundary appropriate to the audited codebase.
- Create foundational records for settings, sessions, entries, generations, schema migrations, and data-management metadata.
- Classify every stored field as diagnostic, performance, learning, or evaluation data.
- Bind all services to loopback and centralize local configuration without hard-coded machine paths or secrets.
- Add Application Control navigation and initial categories without implementing future phase features early.
- Implement Learning Journal first-run default OFF, explicit confirmation, remembered state, and visible local-storage indicator.
- Implement per-entry `Save this entry for learning` state foundation and transient behavior while Journal is off.
- Add configurable retention foundations and protected deletion/export service boundaries.
- Add `.env.example` or equivalent documented configuration while keeping actual local configuration ignored.
- Preserve and migrate existing eligible data safely if Phase 00 finds any.

## Out of scope

- Grammar analysis and Sentence Review UI.
- Token/system metrics and diagnostic bundles.
- Microphone and speech-to-text.
- Vocabulary extraction.
- Study report generation.
- PWA or macOS launcher.

## Deliverables

- Migrations and schema documentation.
- Settings and persistence services.
- Initial Application Control UI.
- Learning Journal consent and visible status.
- Retention and deletion foundations.
- Configuration documentation.
- Phase report and learning notes.

## Acceptance criteria

- A clean database can be created and migrated idempotently.
- Migration failure does not silently corrupt existing data.
- SQLite file location is local, configurable, and not inside a known sync folder by default.
- Learning Journal is OFF on first launch.
- Content is not persisted when Journal and per-entry saving are off.
- Enabling Journal requires confirmation and visibly changes storage state.
- Disabling Journal prevents future automatic saves without deleting prior records.
- Protected deletion cannot be triggered accidentally.
- Existing Writer behavior still works.
- Application remains loopback-only and contains no external runtime asset dependency.

## Targeted test plan

- Migration create, rerun, upgrade, failure, and rollback tests.
- Settings defaults and persistence tests.
- Journal OFF/ON and per-entry storage matrix.
- Database path and permission behavior.
- Protected deletion service tests using temporary data.
- Existing Writer smoke test and directly related API tests.

## Manual and privacy validation

- Inspect SQLite after an unsaved request and verify message content is absent.
- Enable Journal, save sanitized content, restart, and verify explicit persisted state.
- Confirm the UI shows when content is stored locally.
- Confirm no external request or CDN asset is introduced.

## Learning deliverables

- Explain SQLite, migrations, repository/service boundaries, consent state, retention, and loopback configuration in English.
- Link explanations to real files and provide safe inspection commands.

## Completion report

Create `reports/phases/01-completion.md`, including migrations, stored fields by data class, tests, privacy evidence, limitations, and rollback.

## Rollback guidance

Document application rollback and database migration rollback or forward-recovery. Back up existing local data before irreversible schema conversion.

## Approval gate

Set Phase 01 to complete and stop at `awaiting_approval`. Do not begin Phase 02 without `Approve Phase 02`.
