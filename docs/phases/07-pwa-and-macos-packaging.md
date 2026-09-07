# Phase 07: PWA and macOS Packaging

- Status: Approved
- Approved: 2026-09-07
- Prompt: `prompts/phases/07-pwa-and-macos-packaging.md`
- Dependencies: Completed Phase 06 and explicit Phase 07 approval
- Governing EDRs: EDR-001, EDR-002, EDR-003, EDR-006

## Objective

Provide a reliable, understandable, one-action local launch experience without weakening the privacy boundary or existing recovery path.

## Approved scope

- Create a locally stored application identity and icon set for browser, PWA, and macOS use.
- Add a web app manifest and installable behavior supported by the audited target browser.
- Ensure any service worker caches only safe application-shell assets and never sensitive prompts, responses, reports, API calls, or user-specific data.
- Create a maintainable macOS launcher appropriate to the audited project.
- On launch, check Ollama, start the backend if needed, wait for health, avoid duplicate backend instances, and open the local UI.
- Provide graceful status and recovery when Ollama, the model, Python environment, port, or permission is unavailable.
- Document install, update, start, stop, log location, uninstallation, and recovery.
- Validate Dock/Application behavior and optional Desktop alias without requiring a menu-bar component.

## Out of scope

- App Store distribution, code-signing/notarization unless explicitly required and approved.
- Automatic model installation or removal.
- External update services.
- Menu-bar application unless separately approved.
- Sensitive-data caching in browser or service worker.

## Deliverables

- Local icon and manifest assets.
- Safe PWA behavior.
- macOS launcher and single-instance handling.
- Health and dependency status experience.
- Install/uninstall/start/stop documentation.
- Phase report and packaging learning notes.

## Acceptance criteria

- The application starts through the documented one-action path.
- Repeated launch does not create duplicate backend instances.
- Server remains bound to loopback.
- Missing dependencies produce clear instructions and do not trigger automatic downloads.
- Service-worker inspection shows no sensitive request or response caching.
- Existing command-line recovery/start path still works.
- Icons display at required sizes without remote assets.
- Shutdown and uninstall instructions do not delete user learning data without explicit action.

## Targeted test plan

- Manifest and asset validation.
- Service-worker cache allowlist and sensitive-route exclusion tests.
- Launcher start, already-running, port-conflict, missing-Ollama, missing-model, and failed-health tests.
- Graceful shutdown and restart tests.
- Loopback-binding and offline-asset checks.
- Existing application end-to-end smoke test.

## Manual and privacy validation

- Install/open through the target browser and macOS launcher.
- Inspect network and cache behavior with sanitized content.
- Restart the workstation only if the user explicitly chooses that validation.
- Verify no external update, analytics, or asset request.

## Learning deliverables

- Explain PWA, manifest, service worker, safe caching, process lifecycle, ports, health checks, single-instance behavior, icons, and macOS launcher structure in English.

## Completion report

Create `reports/phases/07-completion.md` with supported environments, launch tests, cache inspection, privacy validation, known manual steps, and rollback.

## Rollback guidance

Preserve the pre-packaging start command. Document removing PWA/launcher artifacts without removing application data or Ollama models.

## Approval gate

Set Phase 07 to complete and stop at `awaiting_approval`. Do not begin Phase 08 without `Approve Phase 08`.
