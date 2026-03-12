---
name: linear-cf-advance
description: Advance a terminal ChangeFlow issue in Linear to the next valid phase, enforcing MMCF phase order, applicability checks, and PT-aware bottleneck handling.
---

# Linear CF Advance

Use this skill when the user wants to move an existing terminal `ChangeFlow`
issue to its next valid phase in Linear.

Read these delivery docs before acting:

- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md`

## Workflow

1. Read the issue, its current status, labels, recent comments, and existing
   child `PTSubTask` issues when present.
2. Confirm the issue is a terminal `ChangeFlow`.
3. Enforce phase order:
   - `collect -> analyze -> forecast -> decide`
   - `decide -> implement` only if `Applicable=true`
   - `decide -> evaluate` if `Applicable=false`
   - `implement -> evaluate`
4. Determine whether the planned transition can stay implicit or should be
   represented as an explicit `PTSubTask`.
5. In distributed `CF`, set `pt_form=process-based` by default when adjacent
   phases belong to different agents or contours.
6. If a non-trivial transition is or should be explicit:
   - create, find, or update the `PTSubTask`
   - keep the parent issue in the current phase
   - use bottleneck labels only as visibility aids
7. If the transition failed, inspect whether a PT failure policy allows retry
   and reflect the retry state on the `PTSubTask` or explicit gateway trace.
8. Move the parent issue status only when the next phase is validly ready and
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
