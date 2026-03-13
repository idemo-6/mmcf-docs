---
name: linear-cf-execute
description: Execute the current phase of a terminal ChangeFlow issue in Linear, write structured phase comments, and manage PT-aware phase acceptance under MMCF.
---

# Linear CF Execute

Use this skill when the user wants Codex to do the actual work of the current
phase of an existing terminal `ChangeFlow` issue in Linear and leave
structured phase and gate trace in comments.

Read these delivery docs before acting:

- [mmcf-linear-delivery.md](../linear-mmcf/references/mmcf-linear-delivery.md)

## Defaults

1. Codex is the executor by default.
2. The user is `CFOwner`, observer, decision authority, and evaluation
   authority by default unless the issue body explicitly says otherwise.
3. `strict_gated` is the default gate mode.
4. Do not silently collapse `CFOwner` into Linear `assignee`.

## Scope

1. Execute work inside the current phase of an existing terminal issue.
2. Write structured phase comments.
3. Prepare or update the next `PT` trace.
4. Move the parent issue only when the phase is accepted and the transition is
   valid.
5. Support `evaluate` with checks/tests and a recommended `Result`.
6. If terminal closure is requested, follow the same exit discipline as
   `linear-cf-close`.
7. If `repeat` is chosen, create the next sibling flow in the same turn or
   route to `linear-cf-repeat`.

## Command Model

Interpret these user phrases as control commands when they are used explicitly.
If the user gives the same instruction in plain language, map it to the
nearest command.

### State And Policy

- `show_flow_state`
  Show current phase, active gate, blockers, gate mode, and pending authority.
- `set_gate_mode strict_gated|fast_path`
  `strict_gated` is the default.
- `set_next_gate event|approve|claim|approve_claim`
  Declare the expected next transition for the current phase.
- `materialize_gate`
  Force the current transition to be represented as an explicit `PTSubTask`.

### Phase Work

- `work_phase`
  Perform the next work pass inside the current phase.
- `submit_phase`
  Write a structured phase comment as a candidate for acceptance.
- `continue_phase`
  Keep the issue in the same phase and do more work inside that phase.
- `revise_phase`
  Update or supersede the latest phase summary without changing status.

### Gate Control

- `accept_phase`
  Accept the current phase. If the next transition is simple and valid, move
  the parent issue forward. If the next transition is non-trivial, open or
  update the gate and keep the parent in the current phase until the gate is
  resolved.
- `open_gate`
  Open runtime transition trace for the declared next gate.
- `pass_gate`
  Close the active transition successfully and move the parent issue forward.
- `fail_gate`
  Record terminal transition failure. If retry policy is exhausted, route the
  flow toward `evaluate`.
- `retry_gate`
  Record another transition attempt without changing the parent phase.

### Decision And Closure

- `decide_option <id>`
  Select one forecast option in `decide`.
- `set_applicability true|false`
  Record the applicability verdict at or after `decide`.

- `set_result -1|0|+1`
  Record the recommended operational result in `evaluate`.
- `close_flow done|repeat|final|delayed`
  Close the current flow with a valid terminal exit.
- `spawn_repeat`
  Create the next sibling issue when `repeat` has been selected.

## Workflow

1. Read the issue, current status, body, recent comments, labels, linked or
   child `PTSubTask` issues, and relevant repo files.
2. Confirm the issue is a terminal `ChangeFlow`.
3. Infer the active phase from the parent issue status.
4. Work only inside the current phase until the phase result is ready to
   submit.
5. Use phase-local execution discipline:
   - `collect`: gather relevant files, refs, constraints, and unknowns
   - `analyze`: sharpen the task, requirements, assumptions, and risks
   - `forecast`: enumerate options, tradeoffs, and recommendation
   - `decide`: capture the chosen option, applicability, and implementation boundary
   - `implement`: make the change and summarize what was changed
   - `evaluate`: run checks or review, then recommend `Result` and exit
6. Use `submit_phase` to write the current phase summary.
7. If the user wants more work in the same phase, use `continue_phase` rather
   than inventing a backward phase move.
8. When the phase is accepted, prepare the next transition according to the
   declared or inferred `PT` scenario.
9. Keep `PT` semantics separate from phase status. Retry belongs to `PT`, not
   to a backward phase loop.
10. In `decide`, Codex prepares options and recommendation, but the default
   authority verdict belongs to the user.
11. In `evaluate`, Codex runs checks/tests when possible, writes the evaluation
    summary, and recommends `Result` plus exit.
12. If `repeat` is selected, the current issue closes and a new sibling issue
    is created immediately. Never continue the same issue as the next `CF`.

## Rules

1. Never skip phases.
2. Never move backward to an earlier phase.
3. Never model "phase repetition" as a separate status transition.
4. Additional work before phase acceptance stays inside the same phase.
5. Retry and waiting after phase completion belong to `PT`, not to the phase.
6. `work_phase`, `submit_phase`, `continue_phase`, and `revise_phase` do not
   by themselves move the parent issue status.
7. Only `accept_phase`, `pass_gate`, or explicit close actions may move the
   parent issue forward.
8. A short successful `event` transition may stay implicit as comment trace.
9. A materially significant transition should be explicit as `PTSubTask`:
   - `approve`
   - `claim`
   - `approve+claim`
   - process-based handoff
   - retrying
   - terminal failure
   - unusually long bottleneck
10. `repeat` always creates a new sibling `ChangeFlow`.
11. `delayed` never means "reopen this issue later"; late return also creates a
    new flow.
12. After material `CF5` commit, rollback or correction is treated as a new
   corrective flow, not as erasing history.
13. If `Applicable=false` is discovered early, the flow still proceeds through
    `decide` and `evaluate`; it does not jump directly to terminal exit.
14. Do not silently choose `Applicable`, terminal exit, or final authority
    verdict unless the user explicitly delegated it.
15. Do not close the flow while a blocking gate is unresolved.
16. Do not silently rewrite planning or authority blocks unless the user asked.

## Comment Style

1. Prefer short operational comments over long prose.
2. Omit empty bullets instead of filling them with placeholders.
3. Name files, refs, decisions, and blockers explicitly.
4. End each phase comment with the requested verdict or next action.
5. When a phase is still in progress, use `continue` language rather than
   pretending the phase is already accepted.

## Phase Comment Templates

Use compact phase-specific comments for `submit_phase`.

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

## PT Comment Template

Use this compact form for explicit or comment-level gate trace:

```md
## PT: <from_phase> -> <to_phase>
- Scenario:
- Runtime state:
- Attempt:
- Blockers:
- Evidence/refs:
- Requested action: pass_gate | retry_gate | fail_gate
```

## Evaluate Template

Use this compact form in `evaluate`:

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
