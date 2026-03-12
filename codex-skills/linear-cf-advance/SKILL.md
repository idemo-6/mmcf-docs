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

1. Read the issue, its current status, labels, and recent comments.
2. Confirm the issue is a terminal `ChangeFlow`.
3. Enforce phase order:
   - `collect -> analyze -> forecast -> decide`
   - `decide -> implement` only if `Applicable=true`
   - `decide -> evaluate` if `Applicable=false`
   - `implement -> evaluate`
4. Check whether the planned transition is simple `event` or requires
   `approve`, `claim`, or `approve+claim`.
5. If the transition is non-trivial or failed:
   - add or update the relevant bottleneck label
   - write an explicit gateway trace comment
6. Move the issue status only when the next phase is validly ready.

## Rules

1. Never skip `evaluate`.
2. Never move into `implement` without explicit `Applicable=true`.
3. Do not treat a long approval queue as a phase failure by default.
4. `Blocked` is for non-`PT` external blockers.
5. `AwaitingApproval`, `AwaitingClaim`, and `GatewayFailure` are visibility
   labels, not substitutes for the gateway trace.
6. Do not silently rewrite planning `v1.1` fields or body blocks while
   advancing a flow unless the user explicitly asks for a planning update.
