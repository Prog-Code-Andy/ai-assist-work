# Planning Stage 02: EDR Draft Report

## Outcome

Eight proposed Engineering Decision Records were created for review.

## Covered decisions

- Phase-gated execution and immutable accepted decisions.
- Local-only privacy and network boundaries.
- Browser/PWA, FastAPI, Ollama, SQLite, local speech, and separated control UI architecture.
- Data classification, retention, export, deletion, and future dataset boundaries.
- Model tokens, timing, resources, caching, diagnostic bundles, evaluation, and deferred fine-tuning.
- Required English learning documentation and phase checkpoints.
- Opt-in automatic learning capture, per-entry control, and structured grammar explanations for typed and dictated input.
- Dedicated Study Materials reporting with vocabulary, sentence and grammar, summary, printing, and export views.

## Review update

- CPU monitoring now requires system-wide and AI Assist Work/Ollama measurements around each generation.
- Monitoring remains observational and does not control unrelated processes.
- Raw audio deletion is explicitly separated from optional transcript retention.
- Local telemetry is permitted; external telemetry remains prohibited.
- A junior-friendly glossary was added for project terminology.
- Vocabulary uniqueness now uses one canonical word with multiple senses and dated occurrences.
- Study reports now support date and learning filters, HTML/PDF-ready output, CSV, JSON, previews, and a separate settings category.
- Report Output Contract v1 now fixes the visible vocabulary and sentence structures, realistic examples, linked CSV schemas, sorting, empty states, and format behavior.
- Sentence reporting now distinguishes normalized SQLite storage from a flat user-facing CSV: one row per finding repeats Original and Corrected so filtering never removes the learning context.

## Scope protection

- No EDR has been marked Accepted.
- No roadmap or implementation phase was created.
- No application code was created.
- Workstation implementation remains blocked pending user approval.

## Next gate

Review the six proposed EDRs. Correct them if needed, or explicitly approve them before roadmap work begins.
