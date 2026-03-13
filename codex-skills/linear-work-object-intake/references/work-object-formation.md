# Work-Object Formation Reference

Derived from:

- `mmcf-docs/guides/WORK_OBJECT_FORMATION_QUICK_GUIDE_RU.md`

Use this reference to classify a requested new work item before creating
anything in Linear.

## Ladder

Evaluate the request in this order:

1. `DeltaRegistry entry`
2. `Project`
3. `Epic`
4. terminal `ChangeFlow`
5. `PTSubTask`
6. coordination issue

## Fast Tests

### `DeltaRegistry entry`

Choose this when:

1. there is only a fixed `Delta`;
2. triage outcomes are still open;
3. one atomic base intent is not yet stable.

Do not create a Linear issue.

### `Project`

Choose this when the object is a top-level contour that:

1. has its own boundary;
2. lives through time as a separate system;
3. carries child lifecycle contours.

Do not replace a project contour with a normal issue.

### `Epic`

Choose this when the object is a child lifecycle contour that:

1. has its own boundary;
2. has its own lifecycle meaning or exit condition;
3. is expected to contain several meaningful terminal flows.

Do not use `Epic` for a single pass or a large topic without lifecycle meaning.

### terminal `ChangeFlow`

Choose this when the work is one atomic flow:

`collect -> analyze -> forecast -> decide -> implement? -> evaluate`

And when it has:

1. one carrier contour;
2. one base intent line;
3. one honest `CF6`;
4. a credible terminal exit.

### `PTSubTask`

Choose this when the request is a non-trivial transition inside an existing
terminal issue:

1. `approve`
2. `claim`
3. `approve+claim`
4. process-based handoff
5. retrying or failed transition
6. unusually long transition bottleneck

It is not a second terminal flow.

### coordination issue

Choose this when the object is:

1. a checklist;
2. a decision log;
3. an umbrella container;
4. a follow-up collector without one honest `CF6`.

It is a tool-level convenience object, not a canonical work unit.

## Tie-Breakers

1. If the request fits `DeltaRegistry entry`, do not force it into Linear.
2. If unsure between `Epic` and terminal issue, prefer terminal issue unless
   lifecycle semantics are explicit.
3. If unsure between terminal issue and coordination issue, prefer terminal
   issue unless one honest `CF6` is impossible.
4. If unsure about `PTSubTask`, do not create it for a short successful
   `event` transition.
