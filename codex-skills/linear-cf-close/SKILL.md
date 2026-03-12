---
name: linear-cf-close
description: Close a terminal ChangeFlow issue in Linear from evaluate by writing the MMCF CF6 summary and selecting a valid terminal exit.
---

# Linear CF Close

Use this skill when a terminal `ChangeFlow` issue is in `evaluate` and needs a
valid `CF6` summary plus terminal exit.

Read these delivery docs before acting:

- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md`

## Workflow

1. Read the issue, its evidence, and any explicit gateway traces.
2. Build the `CF6 Summary` with at least:
   - `Applicable`
   - `Result`
   - `Failure class`
   - `Active context`
   - `LC phase snapshot`
   - `Evidence refs`
   - `PT trace refs`
   - `PT summary`
   - `Exit decision`
3. Choose the terminal exit by the agreed matrix:
   - `done`
   - `repeat`
   - `final`
   - `delayed`
4. Update `Result` when supported by Linear custom fields.
5. Move the issue out of `evaluate` only after the `CF6 Summary` exists.

## Rules

1. `done` is default only for `Applicable=true`, `Result=+1`, and no immediate
   next flow.
2. `final` and `delayed` are default only for `Applicable=false`, `Result=0`.
3. There is no terminal stop status for unresolved `Result=-1` without an
   immediate next flow in v1.
4. Do not close with `done` while approval, claim, or gateway failure is still
   unresolved.
5. If `repeat` is selected, the next issue must already exist or be created in
   the same turn.
6. Do not silently rewrite planning `v1.1` fields or body blocks while closing
   a flow unless the user explicitly asks for a planning update.
