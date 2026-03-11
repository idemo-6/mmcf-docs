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

---

## 6. Result and evidence

`Result` is not encoded by issue status.

Minimum required result fields:

1. `Applicable`
2. `Result`
3. `active_context`
4. `lc_phase_snapshot`
5. `evidence_refs`
6. `exit_decision`
7. `zero_class` when `Result=0`
8. `next_cf_issue` for `repeat`
9. `return_condition` for `delayed`

The profile recommends storing these in a mandatory `evaluate` comment and
later migrating stable parts into custom fields.

---

## 7. Invariants

1. One terminal issue equals one `ChangeFlow`.
2. `evaluate` is mandatory before any terminal exit.
3. `done` is allowed by default only with `Result=+1`.
4. `repeat` is allowed only if the next sibling issue exists.
5. `final` and `delayed` are allowed by default only with `Result=0`.
6. `Context`, `C_obs`, `C_coord`, `C_meta` are not hierarchy levels.

---

## 8. Metrics note

Because `final` and `delayed` are mapped to Linear `canceled` type, native
completed metrics in Linear will count primarily applicable completed flows.

This is acceptable in the current profile because:

1. applicable completion and inapplicable stop are intentionally separated;
2. `Result=0` remains visible in the execution trace;
3. future analytics may aggregate all terminal exits separately from Linear's
   native completed counters.

---

## 9. References

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
