---
name: linear-version-sync-review
description: Reconcile Epic-level version snapshots in Linear for MMCF/CDM delivery work by reviewing Observed Version, Observed CF index, Active CF issue, and Version sync note against completed CF6 flows.
---

# Linear Version Sync Review

Use this skill when an `Epic` needs version reconciliation rather than normal
phase work on a single terminal issue.

Read these delivery docs before acting:

- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Versioning-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Profile.md`

## Workflow

1. Read the target `Epic` and capture:
   - `Observed version`
   - `Observed CF index`
   - `Active CF issue`, if present
   - `Version sync note`, if present
2. Find the related terminal issues that already reached `CF6`.
3. For each pending/recent flow, check whether a valid `Post-CF version` is
   already known or still marked as pending re-derivation.
4. If a new snapshot is already clear:
   - update the epic `Observed version`
   - update the epic `Observed CF index`
   - clear or rewrite `Version sync note`
5. If the snapshot is still unresolved:
   - keep the existing `Observed version`
   - sharpen `Version sync note` so it names the blocking `CF#N`
6. Update `Active CF issue` only if the epic still has an ongoing relevant flow.

## Rules

1. This skill reconciles epic-level snapshots; it does not rewrite `CF6`.
2. Do not invent `Post-CF version`; use known snapshots only.
3. `Observed CF index` is derived from `Observed version`; do not treat it as
   the sum of epic issues.
4. `Version note` describes the meaning of the current snapshot.
5. `Version sync note` describes pending sync or reconciliation state.
6. If `final` or `delayed` ended with `Version outcome note: no material version change`,
   leave the epic snapshot unchanged by default.
7. If a pending sync depends on future derivation outside Linear, keep the note
   explicit instead of pretending the epic is fully synchronized.
