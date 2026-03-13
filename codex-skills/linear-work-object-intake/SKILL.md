---
name: linear-work-object-intake
description: Classify a requested new work item in Linear as a Project contour, Epic, terminal ChangeFlow, PTSubTask, coordination issue, or upstream DeltaRegistry entry, and create the correct object without collapsing MMCF boundaries.
---

# Linear Work Object Intake

Use this skill when the user wants to create a new task or work item in
Linear, but the correct MMCF work object is not yet fixed.

Read these docs before acting:

- [work-object-formation.md](./references/work-object-formation.md)
- [delta-promotion.md](./references/delta-promotion.md)
- [mmcf-linear-delivery.md](../linear-mmcf/references/mmcf-linear-delivery.md)

## Workflow

1. Read the request and inspect any known related project, epic, terminal
   issue, or coordination issue.
2. Treat `work-object-formation.md` as the primary decision source for
   classification.
3. Classify the requested object using this ladder:
   - `DeltaRegistry entry`
   - `Project`
   - `Epic`
   - terminal `ChangeFlow` issue
   - `PTSubTask`
   - coordination issue
4. State the classification explicitly before creating anything:
   - `chosen object`
   - `why this fits`
   - `why adjacent object types do not fit`
   - `materialization action`
5. Use the object tests below.
6. If the correct object is `DeltaRegistry entry`, do not create a Linear
   issue. State explicitly that the work is still upstream of terminal issue
   materialization.
7. If the correct object is `Project`, create or update a Linear project only
   when the user explicitly wants a new top-level contour and enough project
   data is known. Otherwise say that issue creation would be the wrong object.
8. If the correct object is `Epic`, create a coordination-contour issue with:
   - title: `[EPIC] <contour>: <objective>`
   - no parent issue
   - a compact body with `Type`, `Purpose`, `Why now`, and `Exit condition`
9. If the correct object is terminal `ChangeFlow`, route to
   `linear-cf-intake`.
10. If the correct object is `PTSubTask`, route to `linear-pt-subtask`.
11. If the correct object is coordination issue, create a non-terminal issue
   with:
   - descriptive title without `[CF#N]`
   - a compact body with `Type`, `Purpose`, `Why this is coordination`,
     `Rule of use`, and `Spin-out rule`
12. Link the created object to the correct project, parent epic, or related
    issue when those contours are already known.
13. In the final response, say what object was chosen, what was created, and
    what should happen next if no Linear object was created.

## Object Tests

Use these tests in the same order as the ladder.

### `DeltaRegistry entry`

Choose this when:

1. there is only a fixed `Delta`;
2. triage outcomes like `drop`, `park`, `merge`, `split`, or `promote` are
   still open;
3. one atomic base intent for a terminal flow is not yet stable.

Action:

1. do not create a Linear issue;
2. say that the request is still upstream of terminal materialization.

### `Project`

Choose this when the object is a top-level contour that:

1. has its own boundary;
2. lives through time as a separate system;
3. carries child lifecycle contours.

Action:

1. create or update a Linear project only when the user explicitly wants the
   contour materialized and enough project data is known;
2. otherwise explain that issue creation would be the wrong object.

### `Epic`

Choose this when the object is a child lifecycle contour that:

1. has its own boundary;
2. has its own lifecycle meaning or exit condition;
3. is expected to contain several meaningful `ChangeFlow` issues.

Action:

1. create a Linear issue titled `[EPIC] <contour>: <objective>`;
2. keep it parentless unless the workspace uses another contour above it;
3. use the epic template below.

### terminal `ChangeFlow`

Choose this when the work is one atomic flow:

`collect -> analyze -> forecast -> decide -> implement? -> evaluate`

And when it has:

1. one carrier contour;
2. one base intent line;
3. one honest `CF6`;
4. a credible terminal exit such as `done`, `repeat`, `final`, or `delayed`.

Action:

1. route to `linear-cf-intake`;
2. do not create a coordination issue as a shortcut.

### `PTSubTask`

Choose this when the request is a non-trivial transition inside an existing
terminal issue:

1. `approve`
2. `claim`
3. `approve+claim`
4. process-based handoff
5. retrying or failed transition
6. unusually long transition that has become a bottleneck

Action:

1. route to `linear-pt-subtask`;
2. never treat it as a second terminal flow.

### coordination issue

Choose this when the object is:

1. a checklist;
2. a decision log;
3. an umbrella or follow-up container;
4. a coordination shell without one honest `CF6`.

Action:

1. create a plain non-terminal issue;
2. use the coordination template below;
3. spin out separate terminal flows when an atomic delta becomes clear.

## Rules

1. Evaluate the ladder in order; do not start from "create a task" as an
   assumption.
2. Never create a terminal issue before `PromotionDecision=promote` is
   effectively made.
3. Never create an `Epic` only because a topic is large or important.
4. Never create coordination issue when the work can honestly be handled as one
   terminal `ChangeFlow`.
5. Never create `PTSubTask` unless a parent terminal issue already exists.
6. Do not put `[CF#N]`, phase names, terminal exits, or active bottlenecks in
   epic or coordination titles.
7. If the request really fits `DeltaRegistry entry` or `Project`, say so
   explicitly rather than forcing it into a Linear issue.
8. When in doubt between `Epic` and terminal issue, prefer terminal issue
   unless lifecycle semantics are explicit.
9. When in doubt between terminal issue and coordination issue, prefer terminal
   issue unless one honest `CF6` is impossible.
10. When in doubt about `PTSubTask`, do not create it for a short successful
    `event` transition.
11. `assignee` is not the canonical synonym of `CFOwner`; do not silently
    collapse them in created issue bodies.
12. If the user asked to "just create a task" but the correct object is
    upstream of Linear issue materialization, say so explicitly and stop at the
    classification result.
13. If project or epic boundaries have to be invented from scratch, stop and
    ask for clarification rather than fabricating lifecycle semantics.

## Epic Template

Use this compact form when creating a new epic issue:

```md
## Type
- Epic / child lifecycle contour, not a terminal `ChangeFlow`.

## Purpose
- 

## Why now
- 

## Exit condition
- 
```

## Coordination Template

Use this compact form when creating a coordination issue:

```md
## Type
- Coordination issue / tool-level container, not a terminal `ChangeFlow`.

## Purpose
- 

## Why this is coordination
- 

## Rule of use
- Use this issue for coordination, review notes, and follow-up collection.
- Spin out separate terminal flows when an atomic delta becomes clear.

## Spin-out rule
- Create a separate terminal issue when one atomic `ChangeFlow` becomes clear.
```
