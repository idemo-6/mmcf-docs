---
title: "MMCF: task workflow guide"
date: 2026-03-13
tags: [MMCF, delivery, task-workflow, Linear, Codex, guide]
status: working-draft
---

# MMCF: task workflow guide

## 1. Purpose

This guide gives a simple practical answer to these questions:

- how to run one terminal issue as one `ChangeFlow`;
- how to separate phase work from phase transition;
- how Codex and a human split roles in task workflow;
- how not to confuse `continue_phase`, `retry_gate`, and `repeat`.

This guide is written for `Linear`-like workflow where:

- `Codex` is the executor;
- the human remains `CFOwner`, observer, decision authority, and final
  evaluator unless that authority is explicitly delegated.

---

## 2. Base model

One terminal issue equals one `ChangeFlow`:

`collect -> analyze -> forecast -> decide -> implement? -> evaluate`

Normative points:

1. `evaluate` is mandatory.
2. If `Applicable=false`, the flow still passes through `decide` and
   `evaluate`.
3. `repeat` creates a new sibling flow; the old issue does not continue as the
   same `CF`.

---

## 3. Default roles

Use this default model unless the issue body says otherwise:

1. `Codex` executes the current phase.
2. the user is `CFOwner`;
3. the user is decision authority;
4. the user is evaluation authority;
5. Linear `assignee` is not automatically the same as `CFOwner`.

This gives a simple operational default: Codex does the work and proposes the
next step, while the human gives the authority verdict where needed.

---

## 4. What each phase means

### `collect`

Phase question:

- what actually belongs to the task and which materials are in scope.

Typical outcome:

- files;
- relevant docs;
- constraints;
- unknowns and blockers.

### `analyze`

Phase question:

- what exactly needs to be done and by which criteria the task will count as
  successful.

Typical outcome:

- sharpened task statement;
- constraints;
- assumptions;
- risks.

### `forecast`

Phase question:

- which solution options exist and how they differ.

Typical outcome:

- options;
- tradeoffs;
- recommendation.

### `decide`

Phase question:

- which option is selected and whether it is applicable.

Typical outcome:

- chosen option;
- `Applicable=true|false`;
- implementation boundary.

### `implement`

Phase question:

- what was materially changed.

Typical outcome:

- change list;
- touched files;
- local checks.

### `evaluate`

Phase question:

- what the actual operational result of the flow is.

Typical outcome:

- checks or review;
- `Applicable`;
- recommended `Result`;
- recommended terminal exit.

---

## 5. Gate layer

After every phase, there is a `PT` (`PhaseTransition`).

This means:

1. phase completion is not automatically the same thing as entering the next
   phase;
2. `PT` may wait for approval;
3. `PT` may retry;
4. `PT` may end in terminal failure.

Recommended operational default:

- `strict_gated`

This means:

1. Codex finishes the phase and writes a phase comment;
2. then the `PT` is formed;
3. then authority gives the verdict for moving forward.

A short successful `event` transition may remain implicit as comment trace.
A non-trivial transition is better materialized as an explicit `PTSubTask`.

---

## 6. Phase comments

Every phase should leave a structured comment.

Recommended style:

1. short;
2. operational;
3. without empty placeholders;
4. with explicit `Requested verdict` at the end.

Minimal forms:

### `collect`

```md
## collect
- Scope/files:
- Relevant refs:
- Open unknowns:
- Blockers:
- Requested verdict: continue_collect | accept_phase
```

### `analyze`

```md
## analyze
- Task statement:
- Constraints:
- Assumptions:
- Risks:
- Requested verdict: continue_analyze | accept_phase
```

### `forecast`

```md
## forecast
- Option A:
- Option B:
- Recommendation:
- Tradeoffs:
- Requested verdict: continue_forecast | decide_option <id> | accept_phase
```

### `decide`

```md
## decide
- Chosen option:
- Applicable:
- Decision rationale:
- Implementation boundary:
- Requested verdict: continue_decide | accept_phase
```

### `implement`

```md
## implement
- Changes made:
- Files changed:
- Checks run:
- Open issues:
- Requested verdict: continue_implement | accept_phase
```

### `evaluate`

```md
## evaluate
- Checks / review:
- Evidence refs:
- Applicable:
- Recommended Result: +1 | 0 | -1
- Failure class:
- Recommended exit: done | repeat | final | delayed
- Requested verdict: set_result <...> | close_flow <...>
```

### `PT`

```md
## PT: <from_phase> -> <to_phase>
- Scenario:
- Runtime state:
- Attempt:
- Blockers:
- Evidence/refs:
- Requested action: pass_gate | retry_gate | fail_gate
```

---

## 7. Command model

Use these commands for live workflow:

### State and policy

- `show_flow_state`
- `set_gate_mode strict_gated|fast_path`
- `set_next_gate event|approve|claim|approve_claim`
- `materialize_gate`

### Work inside a phase

- `work_phase`
- `submit_phase`
- `continue_phase`
- `revise_phase`

### Transition control

- `accept_phase`
- `open_gate`
- `pass_gate`
- `retry_gate`
- `fail_gate`

### Decision and closure

- `decide_option <id>`
- `set_applicability true|false`
- `set_result -1|0|+1`
- `close_flow done|repeat|final|delayed`
- `spawn_repeat`

If the user says the same thing in plain language, the operational meaning
should stay the same.

---

## 8. Most important distinctions

### `continue_phase`

Means:

- work in the current phase is still ongoing.

Does not mean:

- a new `CF`;
- a backward status move;
- a transition retry.

### `retry_gate`

Means:

- retry inside an already opened `PT`.

Does not mean:

- repeat the phase itself;
- a new `CF`.

### `repeat`

Means:

- the current flow is closed;
- a new sibling `ChangeFlow` should start immediately.

Does not mean:

- "we will finish the same issue later";
- "go back to the same phase";
- "reopen the old issue".

### `delayed`

Means:

- no immediate next flow is needed;
- a return is expected later under an explicit condition.

That later return still creates a new flow.

---

## 9. Statuses in Linear

Ideal MMCF-compatible status model:

- `Queued`
- `Todo`
- `collect`
- `analyze`
- `forecast`
- `decide`
- `implement`
- `evaluate`
- `done`
- `repeat`
- `final`
- `delayed`

For the current `Linear` workspace, use this operational bridge:

1. `Backlog` = upstream `DeltaRegistry`;
2. `Planning` = collapsed pre-start alias for a materialized terminal `CF`;
3. `In Progress` = active terminal flow work;
4. the exact phase inside `In Progress` should live either in explicit phase
   status or in comment/body trace.

Important:

1. workflow status alias `Planning` is not the same thing as the issue-side
   `Planning` block;
2. planning custom fields such as `Flow Mode / Variativity Target / Decide Policy`
   are also distinct from workflow status `Planning`.

If the workspace does not yet even use this bridge, a temporary mode is acceptable:

1. keep the issue in generic `In Progress`;
2. keep phase state in comments and body trace;
3. move to explicit phase statuses after workflow setup is complete.

This should be treated only as transitional practice.

---

## 10. Recommended minimal loop

1. if the issue is in `Planning`, first decide whether the flow is ready to
   enter active phase work;
2. if the issue is in `In Progress`, identify the exact current phase from
   status or trace;
3. open the issue and identify the current phase;
4. run `work_phase`;
5. write `submit_phase`;
6. if the phase still needs work, use `continue_phase`;
7. if the phase is ready, use `accept_phase`;
8. open and run `PT` if the transition is non-trivial;
9. in `decide`, record `decide_option` and `Applicable`;
10. in `evaluate`, record `Result` and choose terminal exit;
11. if an immediate next pass is needed, use `repeat` and create a new sibling
   flow.

---

## 11. What not to do

1. do not skip `evaluate`;
2. do not use "phase repetition" as a separate status loop;
3. do not mix `phase state` and `PT state`;
4. do not treat approval delay as automatic failure of the previous phase;
5. do not reopen the old issue instead of creating a new `repeat` flow;
6. do not collapse `CFOwner` into one `assignee`.

---

## 12. References

- [WORK_OBJECT_FORMATION_QUICK_GUIDE_RU](./WORK_OBJECT_FORMATION_QUICK_GUIDE_RU.md)
- [VERSIONING_QUICK_GUIDE_RU](./VERSIONING_QUICK_GUIDE_RU.md)
- [methodology/delivery/MMCF-Delivery-Linear-Profile](../methodology/delivery/MMCF-Delivery-Linear-Profile.md)
- [methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract](../methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile](../methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
