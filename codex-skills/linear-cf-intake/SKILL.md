---
name: linear-cf-intake
description: Create a new terminal ChangeFlow issue in Linear under an existing epic, using the MMCF delivery naming, body template, labels, and PT planning rules.
---

# Linear CF Intake

Use this skill when the user wants a new terminal `ChangeFlow` issue created in
Linear under an existing epic.

This skill is project-specific. It assumes the delivery docs live at:

- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md`

## Workflow

1. Read the parent epic and confirm the new issue is a terminal `ChangeFlow`,
   not a new epic or project.
2. Build the title using:
   - `<Contour>: <base objective> [CF#N]`
3. Set issue metadata:
   - parent epic
   - project/team
   - labels
   - `LC Phase Snapshot`, `Artifact Type`, `Claim Mode` when supported
   - `Flow Mode`, `Variativity Target`, `Decide Policy` when
     the workspace uses planning `v1.1`
4. Build the body from the terminal issue template:
   - `Canon`
   - `Delivery`
   - `Planning` when planning `v1.1` is in scope
   - `PT / Gateways`
   - `Evidence`
5. Predeclare non-trivial transitions only when needed:
   - `approve`
   - `claim`
   - `approve+claim`
6. Create the issue in `Todo` unless the user explicitly requests another
   status.

## Rules

1. One issue equals one `ChangeFlow`.
2. Do not put phase names, terminal exits, or active bottlenecks in the title.
3. Use labels only for issue-level visibility, not instead of status or `PT`
   trace.
4. Do not set `Result` during intake. It is filled only in `evaluate`.
5. If Linear MCP cannot set a required custom field, keep the body correct and
   call out the missing field update in the final response.
6. When planning `v1.1` is used, keep top-level planning custom fields and the
   body `Planning` block semantically aligned.
