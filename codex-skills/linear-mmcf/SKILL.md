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
3. `collect/analyze/forecast/decide/implement/evaluate`;
4. `done/repeat/final/delayed`;
5. `PhaseTransition`, gateway, approval, or claim bottlenecks;
6. claim-aware software or documentation delivery in Linear;
7. task-side planning-aware intake or repeat work using `Flow Mode`,
   `Variativity Target`, or `Decide Policy`;
8. version-aware work using `Observed Version`, `CF Index`, `Pre-CF Version`,
   or `Post-CF Version`;
9. epic-level version reconciliation using `Version sync note`,
   `Observed CF index`, or `Active CF issue`.

This skill is the entry point. It routes work to the narrower operational
skills when needed.

Canonical references:

- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/methodology/delivery/MMCF-Delivery-Codex-Skills-Profile.md`

Operational skill sources:

- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/codex-skills/linear-cf-intake/SKILL.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/codex-skills/linear-cf-advance/SKILL.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/codex-skills/linear-cf-close/SKILL.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/codex-skills/linear-cf-repeat/SKILL.md`
- `/Volumes/WORK/Project/idemo_docs/mmcf-docs/codex-skills/linear-version-sync-review/SKILL.md`

## Routing rules

1. If the task is to create a new terminal issue under an existing epic, read
   and follow `linear-cf-intake`.
2. If the task is to move an existing flow to the next valid phase or handle an
   active gateway bottleneck, read and follow `linear-cf-advance`.
3. If the task is to write `CF6`, set `Result`, or choose `done/repeat/final/delayed`,
   read and follow `linear-cf-close`.
4. If the task is to start the next sibling flow after `repeat`, read and
   follow `linear-cf-repeat`.
5. If the task is to reconcile epic-level `Observed version` or clear/update a
   `Version sync note`, read and follow `linear-version-sync-review`.

If the task spans several operations, execute them in this order:

1. intake
2. advance
3. close
4. repeat

## Shared rules

1. One terminal issue equals one `ChangeFlow`.
2. Issue status is the phase layer only.
3. `PhaseTransition` trace stays separate from status.
4. Labels are visibility aids, not replacements for status or gateway trace.
5. Titles encode the stable flow objective and repeat-chain index, not phase or
   bottleneck state.
6. Planning `v1.1` is additive: intake populates planning fields when the
   workspace uses them, repeat preserves them by default, and advance/close do
   not silently rewrite them unless the user explicitly asks.
7. Versioning in Linear is entity-centric: `Epic` and terminal issue store
   snapshots/windows of carrier entity version, not their own autonomous
   work-item versions.
