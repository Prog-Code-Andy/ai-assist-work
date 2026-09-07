# Phase Prompt 07: PWA and macOS Packaging

Execute this prompt only through `prompts/MASTER.md` after Phase 06 is complete and `Approve Phase 07` is validated.

## Required specification

Read and implement only `docs/phases/07-pwa-and-macos-packaging.md`.

## Execution instructions

1. Add local icons, manifest, safe PWA behavior, and a maintainable macOS launcher appropriate to the audited environment.
2. Never cache prompts, responses, reports, API requests, or user data in the service worker.
3. Keep loopback binding and the existing command-line recovery path.
4. Prevent duplicate backend processes and handle missing Ollama, model, port, environment, or health state clearly.
5. Do not install/remove models, add external update services, or implement unapproved App Store/signing/menu-bar scope.
6. Validate install, start, repeat start, failure recovery, stop, and uninstall behavior without deleting learning data.
7. Produce the report and English learning documentation, update state, request `Approve Phase 08`, and stop.
