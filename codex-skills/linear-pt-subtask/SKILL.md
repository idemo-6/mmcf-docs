---
name: linear-pt-subtask
description: Create or manage explicit PTSubTask issues in Linear for non-trivial MMCF PhaseTransition handling under a parent terminal ChangeFlow issue.
---

# Linear PT Subtask

Use this skill when a non-trivial `PhaseTransition` inside a parent terminal
`ChangeFlow` should be tracked as its own explicit Linear child issue.

Read these delivery docs before acting:

- [mmcf-linear-delivery.md](../linear-mmcf/references/mmcf-linear-delivery.md)

## Workflow

1. Read the parent issue and confirm it is a terminal `ChangeFlow`.
2. Confirm the transition is materially significant and should not stay only as
   a label/comment/body note.
3. Determine or confirm:
   - `template_id` when selected
   - `template_family` when selected
   - `from_phase`
   - `to_phase`
   - `pt_class`
   - `pt_form`
   - `pt_scenario`
   - `pt_runtime_state`
   - `pt_failure_policy` when relevant
   - `attempt_no` when relevant
   - `TransitionOwner`
   - `TransitionExecutor` when the transition is process-based
   - resolving authority refs when the transition is approve-bearing
4. Create or update a child issue under the parent flow using:
   - title: `PT: <from_phase> -> <to_phase> / <template_id or pt_scenario>`
   - a compact body with transition-specific fields only
5. Keep the parent issue in the current phase until the `PTSubTask` is
   successfully closed or otherwise yields a valid authority verdict.
6. If the transition closes successfully, update the child issue state and
   leave clear trace on the parent issue.
7. If the transition fails terminally, keep the failure semantics on the child
   issue and reflect the effect on the parent flow without inventing a hidden
   phase move.

## Rules

1. `PTSubTask` is not a terminal `ChangeFlow`.
2. Never use `collect/analyze/forecast/decide/implement/evaluate` as the status
   model of a `PTSubTask`.
3. Use `PTSubTask` only for non-trivial transitions:
   - `approve`
   - `claim`
   - `approve+claim`
   - process-based handoff
   - retrying or failed transitions
   - unusually long transitions that became their own bottleneck
4. In distributed `CF`, if adjacent phases belong to different agents or
   contours, treat the transition form as `process-based` by default.
5. A short successful `event` transition should remain implicit by default.
6. `PTSubTask` may have its own owner/assignee/history, but it does not become
   a second parent flow or a second atomic work unit.
7. Closing a `PTSubTask` does not by itself rewrite the parent issue into a new
   phase; phase advancement must still be explicit on the parent flow.
8. If the workspace cannot represent a dedicated PT status model, use generic
   issue status plus a structured body block for `PT Runtime State`.
9. If the parent issue already stores a selected PT template binding, use it as
   the default source before inventing ad hoc PT metadata.
10. `PTSubTask` assignee or owner never replaces parent `CFOwner`; the parent
    issue remains the source of truth for flow-level ownership.
11. If transition verdicts expose an active `authority-conflict`, reflect that
    on the parent issue trace and use `AuthorityConflict` only as a visibility
    label on the parent flow.
