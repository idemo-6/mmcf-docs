---
name: linear-cf-repeat
description: Create the next sibling ChangeFlow issue in Linear from an evaluated source flow, preserving the contour, repeat-chain numbering, and carry-over intent.
---

# Linear CF Repeat

Use this skill when an evaluated terminal `ChangeFlow` must continue
immediately as a new sibling flow.

Read these delivery docs before acting:

- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md`

## Workflow

1. Read the source issue and confirm it is ready for `repeat`.
2. Derive the next title using:
   - `<Contour>: <base objective> [CF#(N+1)] / <delta from previous CF>`
3. Preserve:
   - parent epic
   - base task ref
   - carrier entity
   - version source ref
   - contour prefix
   - artifact type
   - claim mode
   - planning custom fields when present
   - `Planning` block when present
4. Sharpen the next flow using:
   - `Repeat reason`
   - `Summary`
   - failed or partial evidence from `CF6`
5. Create the next issue as a new sibling, not as a reopen of the old issue.
6. Link the flows and update the source issue so its `CF6 Summary` references
   the new issue.
7. When the carrier entity is versioned, carry forward:
   - `CF index = previous + 1`
   - `Input version = previous Post-CF version` when known

## Rules

1. `repeat` closes the current flow; it never continues the same issue.
2. The next title keeps the same stable contour and base objective.
3. Only add a `/ <delta>` suffix when it materially sharpens the next flow.
4. Carry over relevant evidence and constraints, but do not copy stale status
   or bottleneck labels blindly.
5. If the source issue lacks enough `evaluate` context to define the next flow,
   do not guess; first complete or clarify `CF6`.
6. Preserve the planning contour by default and revise it only when the repeat
   delta explicitly changes the next flow's planning assumptions.
7. Do not preserve stale version snapshots blindly; if the source `Post-CF version`
   is still unknown, mark the next `Input version` as pending re-derivation.
