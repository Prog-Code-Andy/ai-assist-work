# Workstation Prompt 003: Inspect Stuck Browser Transcription

## Authorization

- Application changes authorized: No
- Control repository changes authorized: No
- Test execution authorized: No
- Phase approval included: No
- Expected result: Read-only microphone-to-transcript code-path diagnosis

## Observed evidence

- Chrome microphone permission is enabled and recently used for the local application.
- Recording visibly starts and Stop changes the UI to `Transcribing...`.
- The Original text field remains empty.
- Chrome Network with `Keep log` and `All` enabled shows only the main document, `app.js`, `health`, `settings`, and `models` requests.
- No audio upload or transcription POST request appears after Stop.

## Prompt to copy into Claude Code

```text
Do not modify any files.
Do not run tests.
Do not start or approve another phase.

Use `.project/workstation-config.local.json` from the control repository to resolve the application repository.

Inspect the existing microphone and transcription implementation in the application repository. Trace the complete code path from:

microphone button → recording start → Stop → recording completion → transcription → insertion into the Original text field

Existing browser evidence shows that microphone permission is enabled and used, but after Stop the UI remains in `Transcribing...`, Original text remains empty, and Chrome Network shows no audio upload or transcription POST request.

Report only:

1. Whether the frontend uses:
   - `SpeechRecognition` or `webkitSpeechRecognition`;
   - `MediaRecorder`;
   - another browser API.
2. The exact application files, functions, event handlers, and line numbers involved.
3. What starts and stops microphone capture.
4. What changes the UI state to `Transcribing...`.
5. Whether recorded audio is created, and its expected MIME type and lifecycle.
6. Whether any recorded audio or transcript request is sent to FastAPI.
7. The exact backend transcription endpoint, or a clear statement that none exists.
8. Every result, error, timeout, stop, and end handler in the transcription path.
9. Why no transcription POST appears in Chrome Network after Stop.
10. Why the UI can remain indefinitely in `Transcribing...`.
11. Whether the implementation performs speech recognition completely locally or may depend on a browser/remote speech service.
12. The smallest recommended correction that preserves the approved local-only architecture.

Distinguish confirmed code evidence from hypotheses. Do not infer successful recording from the visible timer or microphone permission alone.

Do not implement the correction. Stop after returning the diagnosis.
```

## Expected stop condition

Claude Code returns an evidence-based diagnosis with file and line references, without editing either repository, running tests, or advancing the phase state.
