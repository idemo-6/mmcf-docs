---
name: linear-cf-intake
description: Create a new terminal ChangeFlow issue in Linear under an existing epic, using the MMCF delivery naming, body template, labels, and PT planning rules.
---

# Linear CF Intake

Use this skill when the user wants a new terminal `ChangeFlow` issue created in
Linear under an existing epic.

Use `linear-work-object-intake` first when it is not yet clear whether the new
object should be terminal `ChangeFlow`, `Epic`, coordination issue, `PTSubTask`,
or should stay upstream as `DeltaRegistry` work.

Read these delivery docs before acting:

- [mmcf-linear-delivery.md](../linear-mmcf/references/mmcf-linear-delivery.md)

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
   - `Delivery`
   - `Roles / Authority`
   - `Planning` when planning `v1.1` is in scope
   - `PT / Gateways`
   - `Evidence`
5. Populate the `Roles / Authority` block with at least:
   - `CFOwner`
   - `Current assignee / active executor`
   - `Risk class`
   - `Decision authority` and `Evaluation authority` when the flow is not explicitly `low-risk`
6. Predeclare non-trivial transitions only when needed:
   - `approve`
   - `claim`
   - `approve+claim`
7. Materialize a `PTSubTask` during intake only when a transition is already
   known to require its own owner, queue, approval trail, or long-running
   process at creation time.
8. Apply the current workspace status bridge:
   - `Backlog` remains upstream `DeltaRegistry`, not a terminal issue status
   - create the terminal issue in `Planning` by default as the current
     collapsed pre-start alias of `Queued/Todo`
9. Move directly to `In Progress` only when immediate admission to active phase
   work is explicit.

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
11. `assignee` is not the canonical synonym of `CFOwner`; always keep both
    roles readable in the body.
12. For non-`low-risk`, claim-bearing, or governance-heavy flow, do not omit
    the authority entries from `Roles / Authority` during intake.
13. In the current workspace bridge, do not create terminal issues into
    `Backlog`; `Backlog` belongs to upstream delta handling.
14. `Planning` as a workflow status alias is distinct from the issue-side
    `Planning` block and planning `v1.1` fields.
