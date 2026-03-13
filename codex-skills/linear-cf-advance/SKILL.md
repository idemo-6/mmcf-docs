---
name: linear-cf-advance
description: Advance a terminal ChangeFlow issue in Linear to the next valid phase, enforcing MMCF phase order, applicability checks, and PT-aware bottleneck handling.
---

# Linear CF Advance

Use this skill when the user wants to move an existing terminal `ChangeFlow`
issue to its next valid phase in Linear.

Read these delivery docs before acting:

- [mmcf-linear-delivery.md](../linear-mmcf/references/mmcf-linear-delivery.md)

## Workflow

1. Read the issue, its current status, labels, recent comments, and existing
   child `PTSubTask` issues when present.
2. Confirm the issue is a terminal `ChangeFlow`.
3. Enforce phase order:
   - canonical order: `Queued -> Todo -> collect -> analyze -> forecast -> decide`
   - current workspace bridge: `Todo -> In Progress`, with the exact active
     phase tracked either by explicit phase statuses or by comment/body trace
   - `decide -> implement` only if `Applicable=true`
   - `decide -> evaluate` if `Applicable=false`
   - `implement -> evaluate`
4. Treat `Todo` as the current workspace alias of collapsed pre-start
   `Queued/Todo`; do not treat `Backlog` as terminal issue pre-start.
5. Read the `Roles / Authority` block and confirm `CFOwner`, current assignee,
   and relevant authority lines are still readable for the next move.
6. If the issue is in generic `In Progress`, infer the exact current phase from
   explicit phase status when present, otherwise from the latest phase
   comment/body trace before advancing.
7. Determine whether the planned transition can stay implicit or should be
   represented as an explicit `PTSubTask`.
8. In distributed `CF`, set `pt_form=process-based` by default when adjacent
   phases belong to different agents or contours.
9. If a non-trivial transition is or should be explicit:
   - create, find, or update the `PTSubTask`
   - keep the parent issue in the current phase
   - use bottleneck labels only as visibility aids
10. If the transition failed, inspect whether a PT failure policy allows retry
   and reflect the retry state on the `PTSubTask` or explicit gateway trace.
11. If advancing would cross a role boundary, add or update explicit handoff
    trace instead of silently collapsing the change into an assignee switch.
12. Move the parent issue status only when the next phase is validly ready and
   any blocking `PTSubTask` is successfully closed or has produced the required
   authority verdict.

## Rules

1. Never skip `evaluate`.
2. Never move into `implement` without explicit `Applicable=true`.
3. Do not treat a long approval queue as a phase failure by default.
4. `Blocked` is for non-`PT` external blockers.
5. `AwaitingApproval`, `AwaitingClaim`, and `GatewayFailure` are visibility
   labels, not substitutes for `PTSubTask` or gateway trace.
6. `PTSubTask` is not a second `ChangeFlow`; never use phase statuses as the
   primary state model of the transition object.
7. If PT retry policy is still active, keep the issue in the current phase and
   do not force `evaluate` yet.
8. Only terminal PT failure should be treated as a reason to send the flow
   toward `CF6`.
9. Do not silently rewrite planning `v1.1` fields or body blocks while
   advancing a flow unless the user explicitly asks for a planning update.
10. If a selected PT template exists on the parent issue, treat it as the
    default operational shape of the transition before inventing a new one.
11. Do not advance into `implement`, `evaluate`, or close a critical `PT`
    while an unresolved `AuthorityConflict` remains active.
12. `AuthorityConflict` is a visibility label only; the actual resolution must
    live in parent issue comments or gateway trace.
13. Do not silently convert a `CFOwner` handoff into a plain assignee update.
14. If the current workspace collapses active phases into generic `In Progress`,
    status movement alone does not prove phase advancement; confirm the phase
    trace in comments/body.
15. Do not advance a terminal issue out of `Backlog`; in the current workspace
    bridge that object still belongs to upstream delta handling.
