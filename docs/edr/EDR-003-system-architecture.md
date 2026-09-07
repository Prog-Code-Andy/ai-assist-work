# EDR-003: Workstation Application Architecture

- Status: Accepted
- Date: 2026-09-05
- Accepted: 2026-09-06
- Supersedes: None
- Amends: None

## Context

The product needs a maintainable local interface for professional writing, English learning, vocabulary, reports, voice input, and local-model analysis. It must run efficiently on a macOS workstation with Ollama while remaining understandable as a learning project.

## Decision

The target architecture is:

```text
Local browser/PWA UI
        ↓ loopback
Local FastAPI application service
        ├── Ollama native local API
        ├── SQLite application database
        ├── Structured local diagnostic logs
        ├── Local speech-to-text engine
        └── Local HTML/PDF-ready report generation
```

Architectural rules:

1. Use a lightweight web UI. A framework requires a documented benefit; otherwise prefer simple HTML, CSS, and JavaScript.
2. Use FastAPI as the local application boundary and orchestration service.
3. Use the native Ollama local API for installed-model discovery and streaming generation.
4. Default professional-writing requests for `qwen3.5:4b` to non-thinking mode. Thinking activates only through explicit user selection.
5. Use SQLite as the primary structured data store, with migrations and foreign-key integrity.
6. Use SQLite FTS only when full-text search is required. Vector search and embeddings are deferred until a later approved phase demonstrates the need.
7. Speech transcription must remain local. The concrete engine is selected after a workstation capability audit.
8. The main workspace and application-control interface are visually separated.
9. Primary workspace areas are Writer, Study Materials, and Analytics. Study Materials contains separate Vocabulary, Sentences and Grammar, Report Builder, and Downloads views.
10. Application Control is opened separately from the writing workspace and contains General and Models, Privacy and Learning Journal, Voice and macOS Permissions, Learning and Vocabulary, Reports and Export, Logs and Diagnostics, Performance, Data Management, and Advanced settings categories.
11. A later packaging phase may add a PWA identity and macOS launcher without changing the local service boundary.
12. This repository contains documentation and execution controls only. Application code is created in the designated workstation repository.

## Consequences

- The system remains small, inspectable, and compatible with local Ollama.
- SQLite supports reporting and structured learning data without premature vector infrastructure.
- Browser microphone permissions and macOS packaging require explicit learning and validation work.
- The separate control interface reduces accidental privacy or diagnostics changes during writing.

## Alternatives considered

- Modify or fork the Ollama desktop application: rejected because upgrades and maintenance would become difficult.
- Desktop-native application as the initial implementation: deferred because it increases early complexity.
- Vector database as the primary store: rejected for the initial product because current queries are relational and analytical.
- Browser storage as the primary database: rejected because it weakens structured reporting, migrations, and portability.

## Compliance

The roadmap and phases must preserve the loopback-only data flow, separate workstation repository, lightweight implementation, SQLite foundation, and separated Application Control interface.

## Related records

- EDR-002: Local-only privacy boundary
- EDR-004: Data storage and retention
- EDR-005: Model observability and evaluation
- EDR-006: Learning process
- EDR-008: Study materials, reporting, and export
