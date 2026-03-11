---
title: "MMCF Delivery INDEX"
date: 2026-03-11
tags: [MMCF, delivery, Linear, software, documentation, profile]
status: working-draft
---

# MMCF Delivery INDEX

## 1. Purpose

This section contains applied delivery profiles for managing software product
work and documentation work in MMCF-compatible operational systems.

The section is intentionally separated from the root `methodology/` layer,
because delivery-specific documents are expected to grow into a larger corpus.

---

## 2. Core mapping

The current delivery ontology is:

1. `Project` -> parent `LifeCycle` instance.
2. `Epic` -> child `LifeCycle` instance relative to `Project`.
3. terminal issue -> one `ChangeFlow`.
4. `Context`, `C_obs`, `C_coord`, `C_meta` -> separate axes of execution,
   not hierarchy levels of work items.

This keeps `LifeCycle` and `ChangeFlow` distinct in operational practice.

---

## 3. Current documents

1. [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
2. [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)

---

## 4. Planned next documents

The next expected profiles in this section are:

1. Software delivery profile.
2. Documentation delivery profile.
3. Scientific documentation delivery profile.
4. Evidence/review profile for `evaluate`.
5. Delivery metrics and observability profile.

---

## 5. Normative basis

- [MMCF-Canonical](../MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](../MMCF-Minimal-Working-Model.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [CDM LifeCycle-6](../../../fcdm-core/theory/cdm/Specifications/LifeCycle-6_v2.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md)
- [CDM SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md)
- [CDM Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md)
