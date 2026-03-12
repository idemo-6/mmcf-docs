---
name: linear-cf-repeat
description: Create the next sibling ChangeFlow issue in Linear from an evaluated source flow, preserving the contour, repeat-chain numbering, and carry-over intent.
---

# Linear CF Repeat

Use this skill when an evaluated terminal `ChangeFlow` must continue
immediately as a new sibling flow.

Read these delivery docs before acting:

- [mmcf-linear-delivery.md](../linear-mmcf/references/mmcf-linear-delivery.md)

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
   - work domain
   - artifact type when present
   - claim mode
   - planning custom fields when present
   - `Planning` block when present
   - `Risk class` and `CFOwner` when they still remain valid for the next flow
4. Sharpen the next flow using:
   - `Repeat reason`
   - `Summary`
   - failed or partial evidence from `CF6`
   - `Authority conflict refs` and `Role handoff refs` when they materially
     affect the next flow contour
5. Create the next issue as a new sibling, not as a reopen of the old issue.
6. Link the flows and update the source issue so its `CF6 Summary` references
   the new issue.
7. When the carrier entity is versioned, carry forward:
   - `CF index = previous + 1`
   - `Pre-CF version = previous Post-CF version` when known
8. Carry forward PT intent only as planning context; do not copy old
   `PTSubTask` objects into the new flow.
9. Create the next issue in `Queued` by default; move directly to `Todo` only
   when immediate admission to active work is explicit.

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
   is still unknown, mark the next `Pre-CF version` as pending re-derivation.
8. Treat `PTSubTask` history as evidence for the next flow, not as a child-work
   object to be cloned.
9. Preserve the `Roles / Authority` block only as the starting contour of the
   next flow; do not blindly copy stale authority conflicts or obsolete owners.
