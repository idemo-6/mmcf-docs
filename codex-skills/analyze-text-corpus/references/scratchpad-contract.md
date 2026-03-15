# Layered Memory Contract

Use this file as the operating contract for multi-file corpus analysis.

## Memory Layers

Use three distinct files or layers:

1. `corpus-context` for durable corpus-level entities and relations;
2. `research-context` for the frame of the current investigation;
3. `scratchpad` for observations, pending items, conflicts, and reconciliation.

Read them in this order:

1. `corpus-context`
2. `research-context`
3. `scratchpad`

Write into `scratchpad` first. Promote upward only after reconciliation.

## Working Folder

Recommended layout:

```text
_codex_corpus_review/
  corpus-context.md
  studies/
    <study-id>/
      research-context.md
      scratchpad.md
      final-report.md
      file-notes/
```

If the corpus only has one active study, a flat variant is acceptable:

```text
_codex_corpus_review/
  corpus-context.md
  research-context.md
  scratchpad.md
  final-report.md
  file-notes/
```

Keep corpus-level memory separate from study-level memory.

## Statuses

Use exactly these statuses in per-file notes and in the scratchpad:

- `IN_CONTEXT`: the file fits the current interpretation and extends it directly.
- `KEY_EVIDENCE`: the file is central support for a major thesis or relation.
- `PENDING`: the file or fragment is not yet placeable but may become relevant.
- `CONFLICT`: the file challenges the current interpretation or uses incompatible assumptions.
- `NOISE`: the file is likely peripheral, duplicate, administrative, or out of scope.

Prefer `PENDING` over premature rejection.

## Corpus Context

Maintain one `corpus-context.md` file with these sections:

1. `Corpus Identity`
2. `Stable Entities`
3. `Stable Relations`
4. `Canonical Terms and Aliases`
5. `Stable Tensions and Conflicts`
6. `Central Clusters`
7. `Peripheral Patterns`
8. `Promotion Log`

Update `corpus-context` only in a promotion pass. Never write directly into it
from the first reading of a single file.

## Research Context

Maintain one `research-context.md` file per study with these sections:

1. `Research Identity`
2. `Main Question`
3. `Secondary Questions`
4. `Scope`
5. `Out of Scope`
6. `Imported Corpus Priors`
7. `Working Hypotheses`
8. `Relevance Criteria`
9. `Expected Output`
10. `Bias Risks`
11. `Change Log`

Update `research-context` only when the question frame, hypotheses, or scope
actually changed.

## Scratchpad

Maintain one `scratchpad.md` file per study with these sections:

1. `Run Scope`
2. `Current Interpretation`
3. `Observations`
4. `Candidate Links`
5. `Pending`
6. `Conflicts`
7. `Proposed Updates to Research Context`
8. `Proposed Updates to Corpus Context`
9. `Reconciliation Decisions`
10. `Next Reads`
11. `Change Log`

This is the primary write target during reading and reconciliation.

## File Notes

Create one note per file in `file-notes/`. Use a stable file name derived from the
relative source path. Keep these sections:

1. `Source`
2. `Status`
3. `Summary`
4. `Relevance to Research Question`
5. `Key Claims`
6. `Entities and Terms`
7. `Links to Existing Context`
8. `Evidence Anchors`
9. `Candidate Promotions`
10. `Unresolved`

If only part of a file is relevant, say so explicitly instead of summarizing the
whole file equally.

## Update Policy

For each file:

1. Read enough to summarize the file faithfully.
2. Write the file note before changing higher-level context.
3. Write observations into `scratchpad.md`.
4. If the file does not fit, add it to `Pending` or `Conflicts`.
5. Revisit earlier pending items whenever a new file introduces a linking concept.
6. Promote only after reconciliation:
   - to `research-context` when the frame changed;
   - to `corpus-context` when the finding is durable across runs.

## Evidence Discipline

- Keep source pointers for every major claim.
- Distinguish direct statements from your own inference.
- Avoid large verbatim excerpts. Use short anchored snippets only when needed.
- Record confidence informally as `high`, `medium`, or `low` if interpretation is uncertain.

## Final Synthesis Rules

Base the final answer on `corpus-context`, `research-context`, and `scratchpad`.
Do not reconstruct the analysis only from working memory.

The final synthesis should answer:

1. What is the main thesis or structure of the corpus?
2. Under what research frame was that conclusion reached?
3. Which files are foundational?
4. Which files are transitional, conflicting, or peripheral?
5. How coherent is the corpus?
6. What remains unresolved?
