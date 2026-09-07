# Report Output Contract

- Contract: Report Output Contract
- Version: 1.0
- Status: Accepted
- Governing decision: EDR-008
- Date: 2026-09-05
- Accepted: 2026-09-06

## Purpose

This contract defines the information users see when generating vocabulary and sentence-and-grammar study materials. It also defines how the same information is represented in HTML, printable PDF output, CSV, and JSON.

The examples below use fictional data and do not represent stored user content.

## Common report header

Every report begins with a visible header:

| Field | Example |
|---|---|
| Report title | Weekly Vocabulary Report |
| Report type | Vocabulary |
| Period | 2026-09-01 through 2026-09-07 |
| Time zone | America/Toronto |
| Generated locally | 2026-09-08 09:15 |
| Input sources | Typed and Voice |
| Active filters | Unique vocabulary; New and Learning |
| Source cutoff | Records saved through 2026-09-08 09:14 |
| Result count | 2 unique words; 5 occurrences |

The report must show its active filters so the user can understand why a record is present or absent. Dates are displayed in the user's local time zone and exported with an unambiguous timestamp.

## Vocabulary Report

### Default screen and HTML/PDF structure

The default result contains one expandable card or table group per canonical vocabulary item. A word is displayed once, while its meanings and occurrences remain nested beneath it.

Visible summary columns:

| Column | Meaning |
|---|---|
| Word | Display form selected for study |
| Russian translation | Translation for the primary filtered meaning |
| Part of speech | Noun, verb, adjective, expression, or another value |
| Context | IT, cybersecurity, corporate, everyday, or another tag |
| Example in English | Natural sentence using this meaning |
| Russian example | Translation of the English example |
| Uses | Number of matching occurrences in the selected period |
| Status | New, Learning, Known, or Ignored |

Expanding a word shows:

- lemma and pronunciation;
- simple English definition;
- all meanings included by the current filters;
- context-specific Russian translation for each meaning;
- original user example when available;
- corrected user example;
- additional English example and Russian translation;
- collocations and synonyms;
- first and last use in the selected period;
- occurrence history when requested;
- confidence or `Needs review` when normalization or sense matching is uncertain.

### Example output

**Weekly Vocabulary Report**  
Period: September 1–7, 2026 · Unique vocabulary: On · Sort: Uses, highest first

| Word | Russian translation | Part of speech | Context | Example in English | Russian example | Uses | Status |
|---|---|---|---|---|---|---:|---|
| investigate | расследовать; изучить проблему | verb | IT / cybersecurity | We need to investigate the authentication failure. | Нам нужно изучить причину сбоя аутентификации. | 3 | Learning |
| recur | повторяться; возникать снова | verb | IT | The issue may recur after the service restarts. | Проблема может возникнуть снова после перезапуска службы. | 2 | New |

Expanded details for `investigate`:

| Field | Value |
|---|---|
| Lemma | investigate |
| Pronunciation | /ɪnˈvestɪɡeɪt/ |
| Simple definition | To examine a problem carefully to discover its cause. |
| Original user example | Can you check this problem? |
| Corrected user example | Could you investigate this issue? |
| Collocations | investigate an issue; investigate the cause; investigate suspicious activity |
| First / last use | 2026-09-01 / 2026-09-06 |

### Uniqueness behavior

- With `Unique vocabulary only` enabled, the screen and HTML/PDF output show one top-level group per canonical lemma.
- Different meanings appear inside the same group rather than as duplicate top-level words.
- With the option disabled, the user can open an occurrence view showing each dated use.
- Usage count represents occurrences and is never inferred from the number of displayed cards.

### Vocabulary CSV package

CSV cannot safely represent nested meanings and occurrences in one simple row. Therefore a complete vocabulary CSV export contains related files:

`vocabulary_items.csv` — one row per unique word:

```text
vocabulary_item_id,word,lemma,language,pronunciation,usage_count,first_used_at,last_used_at,status
```

`vocabulary_senses.csv` — one row per context-dependent meaning:

```text
sense_id,vocabulary_item_id,part_of_speech,definition_en,translation_ru,context,example_en,example_ru,collocations,synonyms,confidence
```

`vocabulary_occurrences.csv` — included when occurrence history is requested:

```text
occurrence_id,vocabulary_item_id,sense_id,entry_id,occurred_at,input_source,original_fragment,corrected_fragment
```

The identifiers preserve relationships without duplicating canonical vocabulary records.

## Sentences & Grammar Report

### Current-entry review versus historical report

The current Writer result and the historical report have different purposes:

- `Sentence Review` in Writer uses one interactive card with Original and Corrected together and selectable findings. It helps the user review the sentence they just entered.
- `Sentences & Grammar Report` is a static historical table designed for fast scanning, printing, filtering, and Excel/CSV use. It uses one row per grammar finding and repeats the full Original and Corrected sentences in every row intentionally.

Database normalization and report readability are separate concerns. SQLite stores one sentence and links many findings through `entry_id`. The report query joins those records and produces a denormalized, self-contained row for each finding.

### Historical report columns

Every visible report row contains:

| Column | Meaning |
|---|---|
| Sentence ID | Stable identifier shared by all findings from the sentence |
| Error number | Position such as 1 of 3 |
| Date | When the learning entry was saved |
| Source | Typed or Voice |
| Original sentence | Complete user sentence or reviewed transcript |
| Corrected sentence | Complete professional corrected sentence |
| Incorrect fragment | Exact phrase requiring attention |
| Corrected fragment | Replacement in the corrected sentence |
| Category | Tense, article, agreement, preposition, and others |
| Rule | Specific grammar rule |
| Explanation | Junior-friendly explanation in English |
| Why this correction | Intended time or context relationship |
| Correct example | Additional natural English example |
| Russian example | Russian translation of the additional example |
| Confidence | Model confidence or Needs review |
| Learning status | New, Reviewing, Understood, or Ignored |

There are no required tabs or expandable controls in the generated report. Original, Corrected, the exact changed fragment, and its explanation remain visible in the same row.

### Example output

**Sentences & Grammar Report**  
Period: September 1–7, 2026 · Source: Typed and Voice · Confidence: Medium and High

| ID / error | Original sentence | Corrected sentence | Incorrect → corrected | Category | Rule and explanation | Example EN / RU | Status |
|---|---|---|---|---|---|---|---|
| S-104 / 1 of 3 | I have restarted the server yesterday, but issue still exist. | I restarted the server yesterday, but the issue still exists. | have restarted … yesterday → restarted … yesterday | Tense | Use Past Simple with a finished past time. `Yesterday` identifies a completed time in the past. | I restarted the service yesterday. / Я перезапустил службу вчера. | Reviewing |
| S-104 / 2 of 3 | I have restarted the server yesterday, but issue still exist. | I restarted the server yesterday, but the issue still exists. | issue → the issue | Article | Use `the` for the specific issue already known in the conversation. | The issue is still occurring. / Проблема всё ещё возникает. | Reviewing |
| S-104 / 3 of 3 | I have restarted the server yesterday, but issue still exist. | I restarted the server yesterday, but the issue still exists. | exist → exists | Agreement | A third-person singular subject takes `-s` in Present Simple. | The problem still exists. / Проблема всё ещё существует. | Reviewing |

The repeated sentences are intentional report presentation, not duplicate database records. If the user filters the CSV to `Category = Tense`, the remaining row still contains the complete original sentence, complete correction, exact change, rule, and example.

### Sentence and grammar CSV

The default user-facing export is one self-contained file named `sentence-learning-report.csv`. It contains one row per grammar finding:

```text
sentence_id,error_number,error_count,occurred_at,input_source,original_sentence,corrected_sentence,user_final_sentence,incorrect_fragment,corrected_fragment,category,rule_name,explanation_en,why_correct_en,example_en,example_ru,confidence,review_state,learning_status,writing_mode,model
```

When one sentence has four findings, the CSV has four rows. `sentence_id`, `original_sentence`, and `corrected_sentence` repeat in those rows so each row remains understandable after sorting or filtering in Excel.

For typed entries, transcript-specific source fields do not apply. For voice entries, `original_sentence` contains the reviewed transcript. A raw transcript is not included in the learning report unless its separate storage was explicitly enabled.

An optional technical data export may still provide normalized `sentences.csv` and `grammar_findings.csv` files for backup or machine processing. It is not the default study report and must be clearly labeled `Normalized data export`.

## Learning Summary

The combined report contains:

1. Factual period totals from SQLite.
2. Most frequent grammar categories and rules.
3. Present, Past, and Perfect tense review when relevant.
4. Selected original-versus-corrected sentence examples.
5. Unique new vocabulary and words selected for practice.
6. Progress compared with the previous equivalent period.
7. Optional local-LLM study guidance and exercises, clearly labeled `AI-generated study guidance`.
8. Links or embedded sections for the full Vocabulary and Sentences & Grammar reports.

## Format behavior

### HTML

- Self-contained and viewable without the backend.
- Includes embedded local styles and no CDN resources.
- Supports expandable detail on screen and expanded detail in print mode.
- Escapes stored text so user content cannot become executable HTML.

### PDF

- Uses the print layout from the self-contained HTML in the initial implementation.
- Displays report title, period, page numbers, and generation timestamp.
- Avoids splitting a vocabulary or sentence card across pages when practical.

### CSV

- Uses UTF-8 encoding.
- Preserves stable identifiers and ISO timestamps.
- Escapes spreadsheet-sensitive values according to the implementation security specification.
- Uses a flat, self-contained sentence-learning report for user study and Excel filtering.
- May additionally offer a clearly labeled normalized multi-file export for backup or machine processing.

### JSON

- Preserves the nested structure: report metadata, canonical vocabulary items, senses, occurrences, sentences, and grammar findings.
- Includes a contract-version field so future readers can interpret the file correctly.

## Empty and uncertain results

- A valid empty report states `No matching learning records for the selected filters`; it does not ask the LLM to invent study material.
- Low-confidence grammar findings are excluded by default unless the user includes them.
- Uncertain vocabulary normalization or sense matching is labeled `Needs review` and is not silently merged.

## Default sorting

- Vocabulary: usage count descending, then word ascending.
- Sentences: most recent first.
- Grammar findings within a sentence: order of appearance in the original sentence.

## Change control

Version 1.0 was accepted with EDR-008 on 2026-09-06. A material change requires a new contract version and a new EDR that amends or supersedes the accepted decision.
