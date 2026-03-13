---
name: linear-mmcf
description: Manage Linear issues using the MMCF/CDM delivery model, including terminal ChangeFlow issues, Project and Epic lifecycle mapping, PT or gateway bottlenecks, CF6 closure, repeat chains, and claim-aware documentation flows.
---

# Linear MMCF

Use this skill when the user wants to manage Linear work with the MMCF/CDM
delivery model rather than with generic Linear conventions.

Typical triggers:

1. terminal `ChangeFlow` issues;
2. `Project = LifeCycle`, `Epic = child LifeCycle`;
3. work-object selection before creating a new issue in Linear;
3. `collect/analyze/forecast/decide/implement/evaluate`;
4. `done/repeat/final/delayed`;
5. `PhaseTransition`, gateway, approval, or claim bottlenecks;
6. claim-aware software or documentation delivery in Linear;
7. task-side planning-aware intake or repeat work using `Flow Mode`,
   `Variativity Target`, or `Decide Policy`;
8. version-aware work using `Observed Version`, `CF Index`, `Pre-CF Version`,
   or `Post-CF Version`;
9. epic-level version reconciliation using `Version sync note`,
   `Observed CF index`, or `Active CF issue`;
10. explicit `PTSubTask`, process-based handoff, `TransitionOwner`, or
    `TransitionExecutor` work inside a parent flow;
11. selected `PT` template bindings on concrete transitions like
    `forecast -> decide = committee-approval`.

This skill is the entry point. It routes work to the narrower operational
skills when needed.

For the exact delivery schema, read:

- [mmcf-linear-delivery.md](./references/mmcf-linear-delivery.md)

Operational skill sources:

- [linear-work-object-intake](../linear-work-object-intake/SKILL.md)
- [linear-cf-intake](../linear-cf-intake/SKILL.md)
- [linear-cf-execute](../linear-cf-execute/SKILL.md)
- [linear-cf-advance](../linear-cf-advance/SKILL.md)
- [linear-cf-close](../linear-cf-close/SKILL.md)
- [linear-cf-repeat](../linear-cf-repeat/SKILL.md)
- [linear-pt-subtask](../linear-pt-subtask/SKILL.md)
- [linear-version-sync-review](../linear-version-sync-review/SKILL.md)

## Routing rules

1. If the task is to create a new work item in Linear and the correct object
   type is not yet fixed, read and follow `linear-work-object-intake`.
2. If the task is to create a new terminal issue under an existing epic and
   work-object classification is already clear, read and follow
   `linear-cf-intake`.
3. If the task is to do the actual work of the current phase, write structured
   phase comments, or manage phase acceptance with explicit command phrases
   like `work_phase`, `submit_phase`, `continue_phase`, `accept_phase`, or
   gate control, read and follow `linear-cf-execute`.
4. If the task is to move an existing flow to the next valid phase or handle an
   active gateway bottleneck without phase execution work, read and follow
   `linear-cf-advance`.
5. If the task is to write `CF6`, set `Result`, or choose `done/repeat/final/delayed`,
   read and follow `linear-cf-close`.
6. If the task is to start the next sibling flow after `repeat`, read and
   follow `linear-cf-repeat`.
7. If the task is to create, update, or close an explicit non-trivial
   `PhaseTransition` child issue, read and follow `linear-pt-subtask`.
8. If the task is to reconcile epic-level `Observed version` or clear/update a
   `Version sync note`, read and follow `linear-version-sync-review`.

If the task spans several operations, execute them in this order:

1. work-object-intake
2. intake
3. execute
4. `PTSubTask` work as needed
5. advance
6. close
7. repeat

## Shared rules

1. One terminal issue equals one `ChangeFlow`.
2. A terminal issue exists only after promotion from the upstream `DeltaRegistry`.
3. In canonical MMCF, pre-start materialized flows use `Queued/Todo`; in the
   current workspace bridge, do not treat `Backlog` as a terminal issue
   status and use `Todo` as the visible pre-start alias instead.
4. Workflow status `Todo` is distinct from planning `v1.1` fields and the
   body `Planning` block.
5. Issue status is the phase layer only.
6. `PhaseTransition` state stays separate from phase status.
7. Non-trivial `PT` may be materialized as a `PTSubTask`; this never creates a
   second `ChangeFlow`.
8. Labels are visibility aids, not replacements for status, `PTSubTask`, or
   gateway trace.
9. `Linear assignee` is the owner of the nearest operational action; it is not
   the canonical synonym of `CFOwner`.
10. `Roles / Authority` in the issue body is the minimum source of truth for
   `CFOwner`, authority map, and risk class when explicit fields are absent.
11. `AuthorityConflict` is used only while an active authority disagreement
    remains unresolved.
12. Titles encode the stable flow objective and repeat-chain index, not phase or
   bottleneck state.
13. Planning `v1.1` is additive: intake populates planning fields when the
   workspace uses them, repeat preserves them by default, and advance/close do
   not silently rewrite them unless the user explicitly asks.
14. Versioning in Linear is entity-centric: `Epic` and terminal issue store
   snapshots/windows of carrier entity version, not their own autonomous
   work-item versions.
