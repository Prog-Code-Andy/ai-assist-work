# AI Assist Work Glossary

This file is a growing, junior-friendly vocabulary for architecture, development, privacy, and local AI. New abbreviations and unfamiliar terms introduced by the project must be added here.

## ADR — Architecture Decision Record

An ADR records one important architecture decision: the problem, the selected solution, alternatives, and consequences.

Example: choosing SQLite instead of a vector database for the first version.

## EDR — Engineering Decision Record

An EDR records a broader engineering decision. It can cover architecture, development process, operations, testing, privacy, or governance.

In AI Assist Work, EDR is the umbrella term. An architecture decision can therefore be stored as an EDR. We use EDR because decisions such as Phase Gate and Learning Process are important engineering rules but are not only software architecture.

```text
ADR: mainly architecture decisions
EDR: architecture + process + operations + engineering policy
```

Neither abbreviation is a program or technology. They are documentation formats that preserve why a decision was made.

## PWA — Progressive Web App

A PWA is a web application that can behave more like an installed desktop application. It still uses web technologies, but it can have an application icon, open in its own window, and use a web app manifest to describe its name, icons, and display behavior.

For AI Assist Work, the PWA would be the local user interface. It would open the FastAPI service on the workstation and would not make Ollama or the Python backend disappear. A separate launcher may still be needed to ensure the local backend is running.

## CDN — Content Delivery Network

A CDN is a network of external servers that distributes files such as JavaScript libraries, fonts, images, or videos from locations close to users.

A normal website might download a font or JavaScript package from a CDN. AI Assist Work prohibits this because even a small external download creates a network connection and may reveal metadata. Required assets must be stored locally.

## Telemetry

Telemetry is operational information collected from a running system. Examples include request duration, error count, token count, CPU use, memory use, and feature usage.

AI Assist Work allows local telemetry for learning, performance analysis, and troubleshooting. It prohibits sending telemetry to external services. Message content is not technical telemetry and must follow the separate Learning Journal policy.

## Observability

Observability is the ability to understand what a system is doing from logs, metrics, and traces. Telemetry is the collected data; observability is the capability created by using that data well.

For AI Assist Work, observability helps explain whether a slow answer was caused by model loading, token volume, system load, memory pressure, configuration, or an error.

## CPU — Central Processing Unit

The CPU executes general-purpose instructions for macOS and applications. High system CPU use from concurrent work can affect response latency, but local models may also use the GPU and shared memory on Apple Silicon. CPU alone therefore cannot explain every performance change.

AI Assist Work will measure aggregate system CPU and its own relevant processes around each model request. It will observe unrelated workload by default, not stop or control other applications.

## LLM — Large Language Model

An LLM is a model trained to process and generate language. In this project, an LLM rewrites rough text, analyzes grammar, and creates explanations while running locally through Ollama.

## API — Application Programming Interface

An API is a defined way for software components to communicate. An API is not automatically an internet or cloud service.

AI Assist Work uses local APIs:

```text
Browser → FastAPI → Ollama
```

These calls remain on the workstation through a loopback address.

## FastAPI

FastAPI is a Python framework for building APIs. In AI Assist Work, it acts as the local coordinator between the browser interface, Ollama, SQLite, speech transcription, and reports.

## Ollama

Ollama is the local runtime that loads and runs language models. AI Assist Work sends prompts to Ollama through its local API and receives generated text and available performance metrics.

## SQLite

SQLite is a relational database stored in a local file. It supports structured tables, relationships, filtering, aggregation, and migrations without requiring a separate database server.

In this project it will store settings, eligible learning entries, grammar findings, vocabulary, model metrics, feedback, and report metadata.

## JSONL — JSON Lines

JSONL is a text format where every line is an independent JSON object. It is useful for structured logs because events can be appended and processed one at a time.

## Retention

Retention defines how long data is kept and when it is deleted. AI Assist Work will have separate retention controls for diagnostics, performance metrics, learning data, and evaluation data.

## Loopback address

A loopback address points back to the same computer. Common examples are `127.0.0.1` and `localhost`.

Binding AI Assist Work to loopback prevents other computers on the network from directly accessing its local service.

## Phase Gate

A Phase Gate is a mandatory stop between implementation phases. Claude completes the approved work, tests it, writes reports and learning notes, updates status, and then waits for explicit approval before beginning the next phase.

## Learning Journal

The Learning Journal is the local, opt-in collection of original inputs, rewrites, grammar findings, vocabulary, examples, and feedback used for personal study. It is separate from diagnostic logs and is off on first launch.

## Report

A report is a generated view of selected stored data. In AI Assist Work, SQLite provides the factual rows and counts, while the local LLM may optionally add a clearly identified explanation or exercise.

## HTML — HyperText Markup Language

HTML is the document format used to structure web pages. A self-contained AI Assist Work HTML report stores its required styles inside the file, opens locally, and does not download assets from a CDN.

## PDF — Portable Document Format

PDF preserves a document's printable appearance. The initial AI Assist Work approach is to generate print-ready HTML and use the browser's `Save as PDF` function locally.

## CSV — Comma-Separated Values

CSV is a simple table format that can be opened in spreadsheet applications. Vocabulary and sentence reports use separate CSV files because their columns are different.

## Timestamp

A timestamp records when an event occurred, such as when a sentence was entered or a report was generated. Timestamps allow reports to filter learning data by day, week, month, or a custom period.

## Lemma

A lemma is the base dictionary form of a word. For example, `restarted` and `restarting` can map to the lemma `restart`.

## Deduplication

Deduplication prevents multiple copies of the same logical item. AI Assist Work keeps one canonical vocabulary item while retaining separate occurrence records for every dated use.

## Occurrence

An occurrence records one place and time where a vocabulary item was used. Occurrences preserve frequency and context without creating duplicate vocabulary cards.

## Contract

A contract is a precise agreement about the structure or behavior that implementation must provide. An EDR explains the decision and its reasons; a report output contract lists the exact fields, relationships, formats, and examples that code must follow.

## Schema

A schema describes the fields in structured data, their meaning, and how records relate. For example, the sentence report schema separates one sentence from its multiple grammar findings by linking them with an `entry_id`.

## Normalized data

Normalized data stores a fact once and links related records by identifiers. In SQLite, one sentence is stored once and several grammar findings reference it through `entry_id`.

## Denormalized report

A denormalized report intentionally repeats selected context to make every exported row understandable by itself. The sentence CSV repeats the complete Original and Corrected sentences for each grammar finding so Excel sorting and filtering do not break their relationship. This repetition exists in the report, not as duplicate source records in SQLite.

## Roadmap

A roadmap orders product outcomes and dependencies. It explains where the product is going, but it is not permission to implement every item immediately.

## Phase specification

A phase specification defines one bounded unit of implementation: its objective, allowed and prohibited work, dependencies, deliverables, tests, acceptance criteria, rollback, learning work, and approval gate.

## Master prompt

The master prompt is the single reusable instruction given to Claude Code. It reads project state and selects exactly one approved numbered phase prompt. It does not approve phases itself.

## Runbook

A runbook is an operational instruction for performing and recovering a repeatable process. The workstation runbook explains repository setup, launching Claude Code, approvals, GitHub synchronization, and recovery after interruption.
