---
title: "MMCF Delivery: Linear Profile"
date: 2026-03-11
tags: [MMCF, delivery, Linear, profile, ChangeFlow, LifeCycle]
status: profile-draft
---

# MMCF Delivery: Linear Profile

## 1. Scope

This document defines how MMCF/CDM delivery entities are mapped into Linear for
software and documentation work.

The profile is tool-specific. It does not redefine the MMCF or CDM canon.

Decomposition and boundary logic for this profile is summarized separately in:

- [MMCF-Delivery-AppliedRules-Integration-Profile](./MMCF-Delivery-AppliedRules-Integration-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)

---

## 2. Canonical mapping

| Linear object | MMCF/CDM mapping | Rule |
|---|---|---|
| `Project` | parent `LifeCycle` instance | Use only when a top-level system contour exists. |
| `Epic` | child `LifeCycle` instance | Use only if the candidate passes `RLC/CC`. |
| terminal issue | one `ChangeFlow` | One issue equals one full `CF1..CF6`. |
| issue comment in `evaluate` | `CF6` trace | Mandatory for result fixation. |
| labels/custom fields | context/governance axes | Do not model these as hierarchy levels. |

---

## 3. Decomposition rules

### 3.1 `Project -> Epic`

An `Epic` is valid only if it is a distinct child lifecycle relative to the
project contour:

1. it has a boundary;
2. it has a lifecycle-like mode of existence;
3. it can host several meaningful `ChangeFlow` instances;
4. it has its own end/transform criterion.

If these conditions are not met, the work stays directly under `Project` as
terminal issues.

### 3.2 `Epic -> terminal issue`

terminal issues are never treated as lifecycle instances in this profile.
They are `ProcessOf(Epic)` and must represent exactly one `ChangeFlow`.

### 3.3 Context boundary

`Context` is not represented as `Epic` in this profile.
External influences stay in fields, labels, and evidence/comments.

---

## 4. Status model for terminal issues

Statuses represent the phase layer of the workflow.

Gateway and `PhaseTransition` observability is defined separately in:

- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)

### 4.1 Linear statuses

Recommended terminal issue statuses:

- `Backlog`
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
- `duplicate`

### 4.2 Linear type mapping

- `Backlog` -> `backlog`
- `Todo` -> `unstarted`
- `collect/analyze/forecast/decide/implement/evaluate` -> `started`
- `done/repeat` -> `completed`
- `final/delayed/duplicate` -> `canceled`

### 4.3 Transition rules

- `Todo -> collect -> analyze -> forecast -> decide`
- `decide -> implement`, if `Applicable=true`
- `decide -> evaluate`, if `Applicable=false`
- `implement -> evaluate`
- `evaluate -> done | repeat | final | delayed`

The `evaluate` phase must not be skipped.

---

## 5. Terminal semantics

- `done` means the current `ChangeFlow` is complete and no next `CF` is needed.
- `repeat` means the current `ChangeFlow` is complete and a next `CF` must be
  created immediately.
- `final` means `Applicable=false`, `Result=0`, and the line is not expected to
  be resumed.
- `delayed` means `Applicable=false`, `Result=0`, and the line is expected to
  return later when context or policy changes.

`repeat` and `delayed` must not be confused:

- `repeat` -> create next issue now;
- `delayed` -> do not create next issue now; store return condition instead.

Default terminal decision matrix:

1. `Result=+1` and no immediate next `CF` -> `done`
2. `Result=+1` and immediate next `CF` -> `repeat`
3. `Result=-1` and immediate corrective/compensating `CF` -> `repeat`
4. `Result=0` and immediate reformulated `CF` -> `repeat`
5. `Result=0` and no immediate next `CF`, but future return is expected ->
   `delayed`
6. `Result=0` and no immediate next `CF`, and no return is planned -> `final`

There is no separate terminal stop status for `Result=-1` without an immediate
next flow in v1. Such cases must remain in `evaluate` until either:

1. a corrective `repeat` issue is created;
2. policy/governance reframes the case into `final` or `delayed` through an
   explicit inapplicability decision.

---

## 6. Result and evidence

`Result` is not encoded by issue status.

Minimum required execution trace fields:

1. `Applicable`
2. `Result`
3. `failure_class`
4. `active_context`
5. `lc_phase_snapshot`
6. `evidence_refs`
7. `pt_trace_refs` when non-trivial or failed transitions occurred
8. `exit_decision`
9. `zero_class` when `Result=0`
10. `next_cf_issue` for `repeat`
11. `return_condition` for `delayed`

These must be present in a mandatory `evaluate` comment.

### 6.1 Custom fields v1

For the first practical Linear schema, only the stable and compact subset is
promoted into custom fields:

1. `LC Phase Snapshot`
   Allowed values: `F1 | F2 | F3 | F4 | F5 | F6`
2. `Result`
   Allowed values: `+1 | 0 | -1`
3. `Artifact Type`
   Allowed values: `software | documentation | scientific-documentation`
4. `Claim Mode`
   Allowed values: `claim-none | claim-optional | claim-required`

These four fields are sufficient for initial filtering and reporting without
forcing high-entropy execution data into rigid field slots.

Field lifecycle note:

1. set `LC Phase Snapshot`, `Artifact Type`, and `Claim Mode` when the issue is
   created;
2. set `Result` only in `evaluate` when the flow outcome is known.

### 6.2 Data that stays outside custom fields in v1

The following items are intentionally not promoted into custom fields in the
first schema version:

1. `CF phase`
   Already encoded by issue status.
2. `exit_decision`
   Already encoded by terminal status.
3. `previous_cf_issue` and `next_cf_issue`
   Better represented by issue links.
4. `return_condition`
   Better stored in the `evaluate` comment because it is usually textual.
5. `active_context`
   Better stored in issue body and `evaluate` comment until the active context
   taxonomy becomes more stable.

### 6.3 Deferred candidates

The following fields may be added later if execution practice shows stable use:

1. `Zero Class`
2. `Canon Compatibility`
3. `Review Trigger`

---

## 7. Labels v1

Labels in this profile are issue-level cross-cutting signals.

They must not replace:

1. phase statuses;
2. terminal exits;
3. explicit `PhaseTransition` trace.

### 7.1 Existing baseline labels

The following existing labels remain valid as baseline issue classification:

1. `Bug`
2. `Feature`
3. `Improvement`
4. `Chore`
5. `Codex`
6. `Triage`
7. `Blocked`

### 7.2 Additional labels for v1

Recommended additional labels:

1. `AwaitingApproval`
   The active bottleneck is an approval or sign-off transition.
2. `AwaitingClaim`
   The active bottleneck is claim/evidence/canon acceptance.
3. `GatewayFailure`
   The latest transition failed and requires explicit recovery.
4. `NeedsEvidence`
   The issue cannot validly pass `evaluate` or claim-bearing transition without
   additional evidence.
5. `Governance`
   The issue requires policy, canon, or governance involvement beyond normal
   phase execution.
6. `ClaimUpdate`
   The issue changes claim-bearing content or has explicit claim-status impact.

### 7.3 Usage rules

1. `Blocked` is reserved for non-`PT` external blockers.
2. `AwaitingApproval`, `AwaitingClaim`, and `GatewayFailure` are issue-level
   visibility aids for the active transition bottleneck.
3. `AwaitingApproval`, `AwaitingClaim`, and `GatewayFailure` do not replace the
   explicit gateway trace defined in the `PhaseTransition` profile.
4. Normally only one active bottleneck label should be used at a time:
   `Blocked`, `AwaitingApproval`, `AwaitingClaim`, or `GatewayFailure`.
5. `NeedsEvidence`, `Governance`, and `ClaimUpdate` may coexist with a
   bottleneck label.

---

## 8. Naming note

Terminal issue titles should follow the naming convention defined in:

- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)

The issue title identifies the stable flow objective and repeat-chain index.
It must not encode phase status, terminal exit, or active `PhaseTransition`
bottleneck.

---

## 9. Invariants

1. One terminal issue equals one `ChangeFlow`.
2. `evaluate` is mandatory before any terminal exit.
3. `done` is allowed by default only with `Result=+1`.
4. `repeat` is allowed only if the next sibling issue exists.
5. `final` and `delayed` are allowed by default only with `Result=0`.
6. `Context`, `C_obs`, `C_coord`, `C_meta` are not hierarchy levels.

---

## 10. Metrics note

Because `final` and `delayed` are mapped to Linear `canceled` type, native
completed metrics in Linear will count primarily applicable completed flows.

This is acceptable in the current profile because:

1. applicable completion and inapplicable stop are intentionally separated;
2. `Result=0` remains visible in the execution trace;
3. future analytics may aggregate all terminal exits separately from Linear's
   native completed counters.

---

## 11. References

- [MMCF-Canonical](../MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](../MMCF-Minimal-Working-Model.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-and-Applicability-Profile](../MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Delivery-AppliedRules-Integration-Profile](./MMCF-Delivery-AppliedRules-Integration-Profile.md)
- [CDM LifeCycle-6](../../../fcdm-core/theory/cdm/Specifications/LifeCycle-6_v2.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [CDM RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md)
- [CDM SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md)
- [CDM Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md)
