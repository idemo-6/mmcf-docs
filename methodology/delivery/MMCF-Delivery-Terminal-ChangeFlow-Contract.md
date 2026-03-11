---
title: "MMCF Delivery: Terminal ChangeFlow Contract"
date: 2026-03-11
tags: [MMCF, delivery, terminal-issue, ChangeFlow, contract]
status: working-draft
---

# MMCF Delivery: Terminal ChangeFlow Contract

## 1. Purpose

This document defines the contract for a terminal delivery issue treated as one
`ChangeFlow`.

The contract applies to software work, documentation work, and scientific
documentation work.

---

## 2. Definition

A terminal issue is a single operational pass of:

`CF1 -> CF2 -> CF3 -> CF4 -> CF5? -> CF6`

where:

- `CF5` is executed only if `Applicable=true`;
- `CF6` is always required before closure.

The issue is not a lifecycle node.
It is a process inside its parent `Epic`.

---

## 3. Intake and body template

### 3.1 Required issue metadata at intake

When the terminal issue is created, set at least:

1. title according to the naming convention;
2. `Parent Epic`;
3. `LC Phase Snapshot`;
4. `Artifact Type`;
5. `Claim Mode`.

`Result` is not an intake field. It is set only in `evaluate`.

### 3.2 Mandatory body fields

Each terminal issue body must contain at least:

1. `Parent Epic`
2. `Base task ref`
3. `Carrier entity`
4. `Base intent`
5. `Active context(s)`
6. `LC phase snapshot`
7. `Expected delta`
8. `Success criteria`
9. `Stop criteria`
10. `Evidence refs`

Recommended issue body template:

```md
## Canon
- Parent Epic:
- Base task ref:
- Carrier entity:
- Base intent:
- Active context(s):
- LC phase snapshot:

## Delivery
- Artifact type:
- Claim mode:
- Expected delta:
- Success criteria:
- Stop criteria:

## PT / Gateways
- Default PT scenario: event
- Non-trivial transitions:
- Approval refs / owners:
- Claim refs / evidence refs:

## Evidence
- Main evidence refs:
- Related specs/docs:
- Constraints:
```

### 3.3 `PT / Gateways` guidance

The `PT / Gateways` block is used to predeclare non-trivial transitions.

If all expected successful transitions are simple `event` transitions, the
section may stay minimal:

- `Default PT scenario: event`
- `Non-trivial transitions: none`

If approval or claim-bearing transfer is expected, declare it explicitly, for
example:

- `forecast => decide: approve`
- `implement => evaluate: approve`
- `analyze => forecast: claim`

This gives the flow an explicit transition plan before bottlenecks appear.

---

## 4. Phase discipline

Mandatory phase order:

- `collect`
- `analyze`
- `forecast`
- `decide`
- `implement` when applicable
- `evaluate`

If inapplicability is detected during `analyze` or `forecast`, the issue still
must pass through:

- `decide` with `Applicable=false`
- `evaluate` with `Result=0`

Only after that may the issue exit through `final` or `delayed`.

Phase completion does not by itself mean a closed handoff to the next phase.
Where approval, claim/evidence acceptance, or explicit handoff tracking is
required, the transition must follow:

- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)

---

## 5. `CF6` closure contract

The `evaluate` phase must end with a structured comment.

The `CF6` comment is a closure summary, not a full phase or gateway journal.
Explicit gateway traces remain separate, but `CF6` must state which failure or
transition layer materially determined the flow outcome.

Minimum template:

```md
## CF6 Summary
- Applicable: true | false
- Result: +1 | 0 | -1
- Failure class: none | phase-logic | applicability | gateway | mixed
- Active context:
- LC phase snapshot:
- Evidence refs:
- PT trace refs:
- PT summary:
- Exit decision: done | repeat | final | delayed
- Zero class: AF_sem | AF_epi | AF_sto | mixed
- Repeat reason:
- Next CF issue:
- Return condition:
- Review date / trigger:
- Summary:
```

### 5.1 `CF6` guidance

1. `Failure class` explains which layer primarily determined the terminal
   outcome:
   - `phase-logic`
   - `applicability`
   - `gateway`
   - `mixed`
   - `none` for straightforward successful completion
2. `PT trace refs` point to explicit gateway traces when:
   - approval was required;
   - claim-bearing transfer was required;
   - any transition failed;
   - a non-trivial transition created the main bottleneck
3. `PT summary` is a short closure note such as:
   - `implicit event transitions only`
   - `implement => evaluate approve completed after owner review`
   - `forecast => decide approval stalled for 9d`
   - `implement => evaluate failed as gateway; handoff not accepted`
4. `Zero class` is filled only when `Result=0`.
5. `Repeat reason` and `Next CF issue` are mandatory only for `repeat`.
6. `Return condition` and `Review date / trigger` are mandatory only for
   `delayed`.

---

## 6. Exit rules

The terminal exit is selected after `CF6` from three independent questions:

1. what `Result` was observed;
2. is a next `ChangeFlow` required immediately;
3. if not immediate, is future return expected or not.

`PhaseTransition` outcomes influence this decision but do not replace it. A
gateway bottleneck or approval delay is not itself a terminal exit.

### 6.1 `done`

Default meaning:

- `Result=+1`
- no next `CF` is required

Allowed by default only when:

1. `Applicable=true`
2. `Result=+1`
3. no immediate next `CF` is required
4. any non-trivial approval or claim-bearing transition required for closure
   has already completed successfully

Do not use `done`:

1. to mean "implementation finished but approval is still pending";
2. to hide a required corrective follow-up;
3. to close a flow with unresolved gateway failure.

### 6.2 `repeat`

Default meaning:

- current `CF` is complete
- next `CF` must be created immediately

Allowed when the current flow is complete and a new sibling flow must start now.

Typical valid combinations:

1. `Applicable=true`, `Result=+1`, but a sharpened follow-up delta is already
   known;
2. `Applicable=true`, `Result=-1`, and a corrective or compensating flow must
   start immediately;
3. `Applicable=false`, `Result=0`, and the issue is immediately reformulated
   into a new flow with a different actionable delta.

PT-aware note:

1. if the principal outcome was `GatewayFailure`, `repeat` is the default exit
   only when recovery starts now in a new flow;
2. `repeat` is not a placeholder for "maybe later".

Mandatory:

- `Repeat reason`
- `Next CF issue`

### 6.3 `final`

Default meaning:

- `Applicable=false`
- `Result=0`
- no planned return

Use `final` when the current flow closes the line without immediate or expected
future continuation.

Typical valid cases:

1. canonical or governance decision established real inapplicability;
2. approval or claim review ended with a stable "do not continue this line"
   decision;
3. the current formulation is rejected without a planned reformulation.

PT-aware note:

1. a failed approval or claim transition does not automatically mean `final`;
2. `final` is correct only when the transition outcome establishes stable
   inapplicability, not just delay.

Mandatory:

- reason for final stop

### 6.4 `delayed`

Default meaning:

- `Applicable=false`
- `Result=0`
- return is expected later

Use `delayed` when the current flow is inapplicable now, but a future trigger
for valid return is known or expected.

Typical valid cases:

1. missing upstream spec freeze;
2. pending governance decision;
3. missing evidence or protocol completion;
4. unavailable dependency, environment, or resource window.

PT-aware note:

1. long approval time alone is not enough for `delayed`;
2. `delayed` is correct only when the flow is being closed because the next
   valid move depends on a later trigger rather than immediate corrective work.

Mandatory:

- `Return condition`
- `Review date / trigger`
- owner of revisit

### 6.5 Decision matrix

Use the following default mapping:

1. `Result=+1` and no immediate next `CF` -> `done`
2. `Result=+1` and immediate next `CF` -> `repeat`
3. `Result=-1` and immediate corrective/compensating `CF` -> `repeat`
4. `Result=0` and immediate reformulated `CF` -> `repeat`
5. `Result=0` and no immediate next `CF`, but future return is expected ->
   `delayed`
6. `Result=0` and no immediate next `CF`, and no return is planned -> `final`

There is no dedicated terminal stop status for `Result=-1` without an
immediate next flow in v1.

Therefore such cases must remain in `evaluate` until one of the following
occurs:

1. a corrective `repeat` issue is created;
2. policy or governance reframes the case into `Applicable=false`, `Result=0`,
   and then closes it as `final` or `delayed`.

---

## 7. Repeat-chain rule

When an issue exits through `repeat`:

1. the current issue is closed;
2. a new sibling issue is created under the same `Epic`;
3. the new issue inherits the same `Base task ref`;
4. the new issue stores the previous issue id and delta for the next pass.

Recommended naming pattern:

### 7.1 Title form

Terminal issue titles should use the stable contour/objective form:

`<Contour>: <base objective> [CF#N]`

Where:

1. `Contour` is the short stable name of the parent lifecycle contour or epic;
2. `<base objective>` is the stable human-readable objective of this flow;
3. `[CF#N]` identifies the concrete `ChangeFlow` in the repeat chain.

Recommended repeat form:

`<Contour>: <base objective> [CF#(N+1)] / <delta from previous CF>`

### 7.2 Naming rules

1. The title identifies the flow objective, not the current phase.
2. Status aliases such as `collect`, `implement`, `evaluate` do not belong in
   the title.
3. `PhaseTransition` scenarios such as approval or claim bottlenecks do not
   belong in the title.
4. `Result`, `Applicable`, `done/repeat/final/delayed`, and active blocker
   labels do not belong in the title.
5. The contour prefix should stay stable across sibling flows in the same epic.
6. The delta suffix after `/` is used only when it materially sharpens the next
   repeated flow.

### 7.3 Examples

Initial flow:

- `CDM Context: провести абляции и сравнение порогов [CF#1]`

Repeated flow:

- `CDM Context: провести абляции и сравнение порогов [CF#2] / recalibrate tau_soft after failed eval`

The current backlog may keep its existing readable titles until an issue enters
active `ChangeFlow` management. The naming convention applies to new issues and
to issues that are explicitly normalized into repeat chains.

---

## 8. Invariants

1. One terminal issue equals one `ChangeFlow`.
2. Terminal exits are post-`evaluate`, not substitutes for `evaluate`.
3. `repeat` creates a new issue; it never reopens the old one.
4. `delayed` also never reopens the old issue; later return creates a new issue.
5. `Result` is a separate runtime axis from terminal status.

---

## 9. References

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-and-Applicability-Profile](../MMCF-Conflict-and-Applicability-Profile.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
