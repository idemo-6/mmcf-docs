# MMCF Linear Delivery Reference

Use this reference for the exact project-specific Linear model.

## 1. Core ontology

1. `DeltaRegistry` = upstream pre-flow layer; terminal issue appears only after
   promote decision
2. `Project` = parent `LifeCycle`
3. `Epic` = child `LifeCycle`
4. terminal issue = one materialized `ChangeFlow`
5. one terminal issue = one `CF1..CF6`
6. `PTSubTask` = one explicit non-trivial `PhaseTransition` instance under a
   parent terminal issue
7. selected `PT` templates bind to concrete `from_phase -> to_phase` on the
   parent issue

## 2. Status model

Pre-start states:

- `Queued`
- `Todo`

Active phase statuses:

- `collect`
- `analyze`
- `forecast`
- `decide`
- `implement`
- `evaluate`

Terminal exits:

- `done`
- `repeat`
- `final`
- `delayed`

Other states:

- `duplicate`

Default flow:

- `Queued -> Todo -> collect -> analyze -> forecast -> decide`
- `decide -> implement` only if `Applicable=true`
- `decide -> evaluate` if `Applicable=false`
- `implement -> evaluate`

## 3. PhaseTransition and gateway

Canonical `CF-PT` classes:

- `unconditional`
- `conditional`

Operational forms:

- `inline`
- `process-based`

Operational scenarios:

- `event`
- `approve`
- `claim`
- `approve+claim`

Rules:

1. issue status is the phase layer only
2. `PhaseTransition` state stays separate from phase status
3. keep `pt_class`, `pt_form`, and `pt_scenario` as separate descriptors of
   the transition
4. in distributed `CF`, if adjacent phases belong to different agents or
   contours, corresponding `pt_form` should be treated as `process-based` by default
5. not every process-based PT needs a `PTSubTask`; use it only for
   operationally visible non-trivial transitions
6. `AwaitingApproval`, `AwaitingClaim`, and `GatewayFailure` are issue-level
   visibility labels only
7. `AuthorityConflict` is an issue-level visibility label for active
   authority disagreement; it does not replace gateway trace or role trace
8. failed PT attempts under active retry policy do not automatically send the
   flow to `evaluate`
9. template catalog lives outside issue objects; parent issue stores selected
   template bindings and `PTSubTask` stores runtime instantiation

### 3.1 PTSubTask

Use `PTSubTask` for:

1. `approve`
2. `claim`
3. `approve+claim`
4. process-based handoff
5. retrying or failed transitions
6. unusually long transitions that became their own bottleneck

Rules:

1. `PTSubTask` is never a terminal `ChangeFlow`
2. phase statuses belong only to the parent terminal issue
3. parent issue stays in the current phase until the blocking `PTSubTask`
   closes successfully or yields the required verdict

Recommended PTSubTask status model:

- `planned`
- `in_transition`
- `awaiting_approval`
- `awaiting_claim`
- `retrying`
- `closed`
- `failed`

### 3.2 PT template bindings

Keep selected template bindings on the parent issue, for example:

1. `forecast -> decide = committee-approval`
2. `implement -> evaluate = ai-authority-approval`

Do not copy the whole template catalog into the issue.

## 4. Labels

Baseline:

- `Bug`
- `Feature`
- `Improvement`
- `Chore`
- `Codex`
- `Triage`
- `Blocked`

MMCF additions:

- `AwaitingApproval`
- `AwaitingClaim`
- `GatewayFailure`
- `NeedsEvidence`
- `Governance`
- `ClaimUpdate`
- `AuthorityConflict`

## 5. Naming

Terminal issue title:

`<Contour>: <base objective> [CF#N]`

Repeat title:

`<Contour>: <base objective> [CF#(N+1)] / <delta from previous CF>`

Do not encode phase, terminal exit, or active bottleneck in the title.

## 6. Intake fields

Set at issue creation:

1. title
2. parent epic
3. `LC Phase Snapshot`
4. `Work Domain`
5. `Artifact Type` when `Work Domain=artifact`
6. `Claim Mode`
7. `CFOwner`
8. `Current assignee / active executor`
9. `Risk class`

Do not set `Result` at intake.

When the flow is not explicitly `low-risk`, also keep visible:

1. `Decision authority`
2. `Evaluation authority`

`Linear assignee` means the current owner of the nearest operational action.
It is not the canonical synonym of `CFOwner`.

For versioned carrier entities, also keep visible in the body:

1. `Carrier entity id`
2. `CF index`
3. `Pre-CF version`
4. `Version source ref`

For explicit `PTSubTask`, keep visible in the body:

1. `from_phase`
2. `to_phase`
3. `pt_class`
4. `pt_form`
5. `pt_scenario`
6. `pt_runtime_state`
7. `pt_failure_policy`
8. `attempt_no`
9. `template_id`
10. `template_family`

### 6.1 Planning extension `v1.1`

When the workspace uses planning `v1.1`, also manage these issue-side planning
signals:

1. `Flow Mode`
2. `Variativity Target`
3. `Decide Policy`

Keep the structured body block below aligned with those top-level fields:

1. `Planning`

Operational boundary:

1. intake populates planning data when the workspace requires it
2. repeat preserves planning data by default unless the new delta changes it
3. advance and close do not silently rewrite planning assumptions
4. executor matching and capability data stay outside the issue

## 7. Body template

```md
## Canon
- Parent Epic:
- Base task ref:
- Carrier entity:
- CF index:
- Pre-CF version:
- Version source ref:
- Base intent:
- Active context(s):
- LC phase snapshot:

## Delivery
- Work domain:
- Artifact type:
- Claim mode:
- Expected delta:
- Success criteria:
- Stop criteria:

## Roles / Authority
- CFOwner:
- Current assignee / active executor:
- Current CFPhaseOwner:
- TransitionOwner(s):
- Decision authority:
- Commit authority:
- Evaluation authority:
- Gateway approval authority:
- Claim authority:
- Risk class:
- Independence note:

## Planning
- Flow mode:
- Variativity target:
- Decide policy:
- max_decide_delay:
- forced_collapse_trigger:
- resource_cap:
- Planning rationale:

## PT / Gateways
- Default PT scenario: event
- Transition templates:
- Non-trivial transitions:
- PT failure policy:
- Negative result policy:
- Approval refs / owners:
- Claim refs / evidence refs:

## Evidence
- Main evidence refs:
- Related specs/docs:
- Constraints:
```

## 8. CF6 summary

```md
## CF6 Summary
- Applicable: true | false
- Result: +1 | 0 | -1
- Failure class: none | phase-logic | applicability | gateway | mixed
- Active context:
- LC phase snapshot:
- Evidence refs:
- Post-CF version:
- Version outcome note:
- PT trace refs:
- PT summary:
- Exit decision: done | repeat | final | delayed
- Closure reason: success | inapplicable | failed | mixed
- Zero class: AF_sem | AF_epi | AF_sto | mixed
- Repeat reason:
- Next CF issue:
- Return condition:
- Review date / trigger:
- Summary:
- CFOwner at closure:
- Authority conflict refs:
- Role handoff refs:
```

## 9. Exit matrix

1. `Result=+1` and no immediate next `CF` -> `done`
2. `Result=+1` and immediate next `CF` -> `repeat`
3. `Result=-1` and policy/verdict chooses immediate corrective flow -> `repeat`
4. `Result=-1` and policy/verdict chooses terminal stop -> `final`
5. `Result=-1` and policy/verdict chooses deferred return -> `delayed`
6. `Result=0` and immediate reformulated `CF` -> `repeat`
7. `Result=0` and no immediate next `CF`, but future return is expected ->
   `delayed`
8. `Result=0` and no immediate next `CF`, and no return is planned -> `final`

If PT failure policy is active:

1. attempt-level failures stay inside the same `PT`
2. only terminal `pt_result=false` sends the flow to `CF6`

## 10. Versioning

Rules:

1. one terminal issue = one `CF#N` for the carrier entity
2. `[CF#N]` in the title must match the body `CF index`
3. `Pre-CF version` is the inherited snapshot before the current `CF#N` and
   may legitimately point to an earlier `cf` segment
4. `Post-CF version`, when derived, should align with the current `CF index`
5. `Epic` holds `Observed version` and optional `Observed CF index`, but this is
   not the sum of epic issues
6. `repeat` should carry forward the same carrier entity and use the previous
   `Post-CF version` as the next `Pre-CF version` when available
7. in the delivery/Linear default, a new material `Post-CF version` is mainly
   expected for `done` and `repeat`
8. for `final` and `delayed` with `Closure reason=inapplicable`, prefer
   `Version outcome note: no material version change`
9. for `final` and `delayed` with `Closure reason=failed`, make the note depend
   on the actual material change rather than assuming no change
10. if you need to show the active ongoing flow on the epic, keep it as a
   separate `Active CF issue`, not inside `Observed CF index`
11. on the epic, use `Version note` for the meaning of the observed snapshot
    and `Version sync note` for pending reconciliation or re-derivation
12. use a dedicated version-sync review pass when an epic has stale snapshot
    data or a non-empty `Version sync note`
