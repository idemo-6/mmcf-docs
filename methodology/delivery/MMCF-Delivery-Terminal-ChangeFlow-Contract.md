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

## 3. Mandatory body fields

Each terminal issue must contain at least:

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

## Success
- Expected delta:
- Success criteria:
- Stop criteria:

## Evidence
- Main evidence refs:
- Related specs/docs:
- Constraints:
```

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

---

## 5. `CF6` closure contract

The `evaluate` phase must end with a structured comment.

Minimum template:

```md
## CF6 Summary
- Applicable: true | false
- Result: +1 | 0 | -1
- Active context:
- LC phase snapshot:
- Evidence refs:
- Exit decision: done | repeat | final | delayed
- Zero class: AF_sem | AF_epi | AF_sto | mixed
- Repeat reason:
- Next CF issue:
- Return condition:
- Review date / trigger:
- Summary:
```

---

## 6. Exit rules

### 6.1 `done`

Default meaning:

- `Result=+1`
- no next `CF` is required

### 6.2 `repeat`

Default meaning:

- current `CF` is complete
- next `CF` must be created immediately

Mandatory:

- `Repeat reason`
- `Next CF issue`

### 6.3 `final`

Default meaning:

- `Applicable=false`
- `Result=0`
- no planned return

Mandatory:

- reason for final stop

### 6.4 `delayed`

Default meaning:

- `Applicable=false`
- `Result=0`
- return is expected later

Mandatory:

- `Return condition`
- `Review date / trigger`
- owner of revisit

---

## 7. Repeat-chain rule

When an issue exits through `repeat`:

1. the current issue is closed;
2. a new sibling issue is created under the same `Epic`;
3. the new issue inherits the same `Base task ref`;
4. the new issue stores the previous issue id and delta for the next pass.

Recommended naming pattern:

- `CF#N: <base objective>`
- `CF#(N+1): <base objective> / <delta from previous CF>`

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
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-and-Applicability-Profile](../MMCF-Conflict-and-Applicability-Profile.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
