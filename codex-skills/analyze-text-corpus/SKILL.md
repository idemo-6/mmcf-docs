---
description: Analyze a large directory or corpus of related text and markdown files by maintaining a durable corpus-context, a question-specific research-context, and a scratchpad for observations and reconciliation. Use when the user asks Codex to review many semantically related documents, theories, notes, specs, or drafts and give an overall conclusion without losing context.
name: analyze-text-corpus
---

# Analyze Text Corpus

Use external on-disk layered memory instead of relying on transient model memory.
Keep three separate layers:

1. `corpus-context` for durable entities and relations in the corpus;
2. `research-context` for the frame of the current investigation;
3. `scratchpad` for observations, pending items, conflicts, and reconciliation.

Read the artifact contract before starting:

- [scratchpad-contract.md](./references/scratchpad-contract.md)

Use the templates when creating the working folder:

- [corpus-context-template.md](./assets/corpus-context-template.md)
- [research-context-template.md](./assets/research-context-template.md)
- [scratchpad-template.md](./assets/scratchpad-template.md)
- [file-note-template.md](./assets/file-note-template.md)
- [final-report-template.md](./assets/final-report-template.md)

## Workflow

1. Define scope before reading:
   - target directory or explicit file list;
   - file types to include;
   - exclusions such as archives, staging mirrors, generated files, or duplicates;
   - the question to answer in the final synthesis.
2. Create or reopen a corpus-review workspace near the work, for example:
   - `_codex_corpus_review/corpus-context.md`
   - `_codex_corpus_review/studies/<study-id>/research-context.md`
   - `_codex_corpus_review/studies/<study-id>/scratchpad.md`
3. Initialize or reuse these artifacts:
   - `corpus-context.md`;
   - `research-context.md`;
   - `scratchpad.md`;
   - `file-notes/`;
   - `final-report.md`.
4. Read the layers in this order before opening corpus files:
   - `corpus-context`;
   - `research-context`;
   - current `scratchpad`.
5. Build a stable inventory with `rg --files` or a filtered file list. Process files
   in a deterministic order.
6. For each file:
   - write a short note using the file note template;
   - extract claims, entities, terminology, dependencies, tensions, and evidence;
   - classify the file or fragment as `IN_CONTEXT`, `KEY_EVIDENCE`, `PENDING`,
     `CONFLICT`, or `NOISE`;
   - write observations and tentative links into `scratchpad.md`, not directly into
     `corpus-context.md`.
7. Every few files, revisit `PENDING` and `CONFLICT` items. Record proposed
   updates to `research-context` and `corpus-context`, but do not promote them
   immediately.
8. Run a promotion pass:
   - update `research-context` if the question frame changed;
   - update `corpus-context` only with durable cross-run findings.
9. Produce the final synthesis from the layered artifacts, not from memory alone.

## Processing Rules

1. Keep each per-file summary compact. Prefer 5-10 lines over long paraphrases.
2. Record source anchors. At minimum, keep the file path and section heading; add
   line references when precision matters.
3. Separate observation from inference. Mark inferred links explicitly.
4. Do not force coherence too early. Use `PENDING` when a file may become relevant
   later, and `CONFLICT` when it pushes against the current frame.
5. Preserve terminology drift. If different files use different terms for a similar
   role, record the mapping instead of normalizing silently.
6. Treat `scratchpad` as the primary write target during reading.
7. Update `research-context` only when the research frame itself changes.
8. Update `corpus-context` only in a separate promotion pass after reconciliation.
9. When the corpus is very large, batch files by cluster, subdirectory, or topic and
   reconcile after each batch.

## Reconciliation Loop

Run a reconciliation pass after each batch and again at the end.

During reconciliation:

- re-read `Pending Fragments` and `Conflicts`;
- merge duplicate entities or thesis variants;
- split overloaded concepts that were incorrectly grouped;
- propose promotions into `research-context` or `corpus-context`;
- leave unresolved items visible rather than deleting them.

## Final Output

Produce a final answer that is grounded in `corpus-context`,
`research-context`, and `scratchpad`, and includes:

- the corpus-level thesis or main interpretation;
- how the current research frame shaped the reading;
- the main supporting clusters of files;
- important contradictions or unresolved branches;
- an assessment of internal coherence;
- the files that are central versus peripheral;
- any promoted or proposed updates to higher-level context files;
- residual uncertainty and what would most reduce it.

If the user asks for a report file, write it into the scratchpad folder using the
report template. Otherwise, keep the working artifacts on disk and summarize the
result in the response.

## Practical Triggers

This skill is a good fit for prompts like:

- "Review the files in this directory and tell me whether they form one theory."
- "Read these drafts and give a unified conclusion without losing the earlier context."
- "Analyze this research corpus and identify the core thesis, contradictions, and outliers."
- "Compare these notes and specs, then tell me what is central and what is peripheral."
- "Analyze this corpus under a specific research question and keep the corpus memory separate from the current hypothesis."

Avoid using this skill for a one-file summary or for a tiny corpus that fits easily
in one pass without an external scratchpad.
