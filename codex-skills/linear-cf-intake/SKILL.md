---
name: linear-cf-intake
description: Create a new terminal ChangeFlow issue in Linear under an existing epic, using the MMCF delivery naming, body template, labels, and PT planning rules.
---

# Linear CF Intake

Use this skill when the user wants a new terminal `ChangeFlow` issue created in
Linear under an existing epic.

Read these delivery docs before acting:

- [MMCF-Delivery-Linear-Profile](../../methodology/delivery/MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](../../methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](../../methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Delivery-Linear-Planning-Profile](../../methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md)

## Workflow

1. Read the parent epic and confirm the new issue is a terminal `ChangeFlow`,
   not a new epic or project.
2. Build the title using:
   - `<Contour>: <base objective> [CF#N]`
3. Set issue metadata:
   - parent epic
   - project/team
   - labels
   - `LC Phase Snapshot`, `Work Domain`, `Claim Mode` when supported
   - `Artifact Type` when `Work Domain=artifact`
   - `Flow Mode`, `Variativity Target`, `Decide Policy` when
     the workspace uses planning `v1.1`
4. Build the body from the terminal issue template:
   - `Canon`
   - `Versioning` when the carrier entity is versioned
   - `Delivery`
   - `Planning` when planning `v1.1` is in scope
   - `PT / Gateways`
   - `Evidence`
5. Predeclare non-trivial transitions only when needed:
   - `approve`
   - `claim`
   - `approve+claim`
6. Materialize a `PTSubTask` during intake only when a transition is already
   known to require its own owner, queue, approval trail, or long-running
   process at creation time.
7. Create the issue in `Todo` unless the user explicitly requests another
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
7. When the carrier entity is versioned, make `CF index` explicit and keep it
   aligned with the title form `[CF#N]`.
8. If a pre-entry snapshot is known, store it as `Pre-CF version`; do not use
   it as a replacement for the current in-flow canonical version.
9. Do not confuse a planned non-trivial transition with a second terminal flow.
   If an explicit transition object is needed, it must be a `PTSubTask`, not a
   sibling `ChangeFlow`.
10. When a reusable PT template is known, record the selected template binding
    on the concrete `from_phase -> to_phase`, rather than writing only a generic
    transition note.
