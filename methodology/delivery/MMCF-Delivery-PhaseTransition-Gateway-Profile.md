---
title: "MMCF Delivery: PhaseTransition and Gateway Profile"
date: 2026-03-11
tags: [MMCF, delivery, PhaseTransition, gateway, ChangeFlow]
status: working-draft
---

# MMCF Delivery: PhaseTransition and Gateway Profile

## 1. Purpose

This document defines the minimum delivery profile for `CF-PhaseTransition`
and gateway observability in Linear-like operational systems.

The goal is practical: separate phase execution time from transition,
handoff, approval, and acceptance time.

---

## 2. Canonical principle

In delivery practice, phase completion and phase transition are not the same.

A transition `CFi => CF(i+1)` is treated as a separate process with its own:

1. result;
2. duration;
3. responsibility;
4. failure mode.

This makes it possible to see whether a bottleneck is in phase execution or in
handoff, approval, or claim/evidence acceptance.

---

## 3. Canonical and operational classification

Canonical `CF-PT` classification remains:

1. `unconditional`
2. `conditional`

For delivery practice, the following operational scenarios are used:

1. `event`
2. `approve`
3. `claim`
4. `approve+claim`

Mapping rule:

1. `event` is typically `unconditional`;
2. `approve`, `claim`, and `approve+claim` are typically `conditional`.

The operational labels do not replace the canonical classification.

---

## 4. Operational scenarios

### 4.1 `event`

Simple transition after phase completion.

Typical meaning:

1. the output of the previous phase is delivered directly;
2. no explicit approval step is required;
3. the next phase may start immediately.

### 4.2 `approve`

Transition requires explicit confirmation, sign-off, or acceptance.

Typical cases:

1. review acceptance;
2. owner approval;
3. release or policy gate;
4. editorial or governance approval.

### 4.3 `claim`

Transition requires a valid claim/evidence/canon bundle before the next phase
may start.

Typical cases:

1. claim-bearing scientific documentation;
2. canonical or alignment documents;
3. software or documentation tasks with explicit claim-bearing outputs.

### 4.4 `approve+claim`

Transition requires both:

1. explicit approval;
2. valid claim/evidence acceptance.

This is common when a claim-bearing output must also pass owner or governance
sign-off.

---

## 5. Minimal trace for v1

For each explicit gateway trace, record at least:

1. `from_phase`
2. `to_phase`
3. `pt_class = unconditional | conditional`
4. `pt_scenario = event | approve | claim | approve+claim`
5. `result = true | false`
6. `t_start`
7. `t_end`
8. `input_ref` and `output_ref`
9. `approval_ref` when approval is required
10. `claim_ref` or `evidence_ref` when claim-bearing transfer is required
11. `failure_reason` when `result=false`

For v1:

1. `approve`, `claim`, `approve+claim`, and failed transitions must be logged
   explicitly;
2. simple successful `event` transitions may remain implicit in the status
   change.
3. for issue-level filtering in Linear, the active transition bottleneck may be
   mirrored by labels such as `AwaitingApproval`, `AwaitingClaim`, or
   `GatewayFailure`.

---

## 6. Responsibility note

Phase responsibility and gateway responsibility stay separate:

1. the phase owner is responsible for the phase output;
2. the gateway owner is responsible for delivery, acceptance, and transition
   validity.

Therefore a long approval queue is a `PhaseTransition` bottleneck, not
automatically a failure of the adjacent phase.

---

## 7. Profile usage

Recommended default by delivery profile:

1. software: `event` by default; `approve` for review/release/security gates;
   `claim` only for claim-bearing outputs;
2. documentation: `event` by default; `approve` for editor/owner/canon gates;
   `claim` only where claim-bearing material exists;
3. scientific documentation: `claim` or `approve+claim` is common, but only
   on transitions that actually carry claim-bearing output.

---

## 8. References

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [CDM PhaseTransition Overview](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition_Overview.md)
- [CDM PhaseTransition-CF](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
