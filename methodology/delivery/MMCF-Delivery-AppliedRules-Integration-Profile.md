---
title: "MMCF Delivery: AppliedRules Integration Profile"
date: 2026-03-11
tags: [MMCF, delivery, AppliedRules, profile, decomposition]
status: profile-draft
---

# MMCF Delivery: AppliedRules Integration Profile

## 1. Scope

This short profile explains how delivery ontology in MMCF maps to CDM
`AppliedRules` without repeating their full content.

The purpose is only to justify the delivery decomposition:

1. `Project`
2. `Epic`
3. terminal issue
4. `Context` and context roles

---

## 2. `RLC/CC`

Use [RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md) to decide whether a candidate object is:

1. a distinct lifecycle-bearing node;
2. or only a process inside its parent contour.

Delivery implication:

- `Project` and `Epic` are used only for lifecycle-bearing contours;
- terminal issue is treated as `ProcessOf(parent)` and represents one
  `ChangeFlow`.

---

## 3. `ROS`

Use [ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md) to stop decomposition when extra hierarchy no longer adds:

1. explanatory power;
2. control value;
3. result stability.

Delivery implication:

- not every work topic becomes an `Epic`;
- decomposition stops at the first operationally sufficient level.

---

## 4. `SEC/OC`

Use [SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md) to separate internal decomposition from external context.

Delivery implication:

- `Context` is not modeled as `Project` or `Epic`;
- external constraints stay in fields, labels, and evidence/comments.

---

## 5. `Observer`

Use [Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md) for context-role interpretation.

Delivery implication:

- `C_obs`, `C_coord`, `C_meta` are execution/governance roles;
- they are not hierarchy levels of work items.

---

## 6. Result

The delivery ontology is therefore:

1. `Project` -> parent `LifeCycle`
2. `Epic` -> child `LifeCycle`
3. terminal issue -> one `ChangeFlow`
4. `Context` and context roles -> separate axes of execution

---

## 7. References

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [CDM RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md)
- [CDM SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md)
- [CDM Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md)
