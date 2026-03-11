---
title: "MMCF Delivery: Software Profile"
date: 2026-03-11
tags: [MMCF, delivery, software, profile, ChangeFlow]
status: profile-draft
---

# MMCF Delivery: Software Profile

## 1. Scope

This profile defines how terminal `ChangeFlow` issues are interpreted in
software product and software platform work.

The profile assumes the Linear mapping defined in:

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)

---

## 2. Carrier and artifact

In software delivery, the carrier entity is typically one of:

1. codebase module;
2. service/subsystem;
3. interface or contract surface;
4. deployment/runtime contour;
5. test or validation contour.

The terminal issue still represents one `ChangeFlow`, not one pull request and
not one file.

Commits, pull requests, migrations, and test runs are implementation/evidence
artifacts of the flow, not the flow itself.

---

## 3. Phase interpretation

### `collect`

Minimum:

1. define the target delta;
2. capture the affected carrier entity;
3. capture relevant constraints and active context.

Typical software evidence:

- bug report;
- failing test;
- stack trace;
- architecture note;
- product requirement;
- regression description.

### `analyze`

Minimum:

1. explain current behavior or failure mode;
2. identify relevant invariants/contracts;
3. reduce ambiguity enough for real alternatives.

Typical outputs:

- root-cause notes;
- affected modules list;
- invariant/contract list;
- failure classification.

### `forecast`

Minimum:

1. produce admissible implementation paths;
2. estimate risk, blast radius, and dependency impact;
3. identify expected verification path.

Typical outputs:

- solution options;
- migration path;
- rollback/compensation note;
- risk comparison.

### `decide`

Minimum:

1. choose one execution path;
2. confirm `Applicable(Intent, C_active, LC_phase)=true` or `false`;
3. freeze the execution branch for this flow.

If `Applicable=false`, the issue still goes through `evaluate`.

### `implement`

Software interpretation:

1. code/config/schema/test fixture change is committed;
2. the chosen path is physically applied to the carrier contour;
3. the implementation is no longer only hypothetical.

Typical artifacts:

- commits;
- pull request;
- migration;
- config change;
- feature flag change.

### `evaluate`

Minimum:

1. confirm runtime result `+1/0/-1`;
2. attach evidence;
3. record whether a next `CF` is needed.

Typical software evidence:

- test results;
- review result;
- runtime checks;
- metrics/logs;
- incident-free verification window;
- regression findings.

---

## 4. Evidence minimum

Recommended minimum evidence bundle for software terminal issues:

1. implementation reference (`commit`, `PR`, or equivalent);
2. verification evidence (`tests`, `review`, `manual check`, `runtime signal`);
3. affected carrier entity;
4. relevant context/risk note;
5. `CF6` summary comment.

For higher-risk work, add:

1. rollback/compensation note;
2. blast-radius note;
3. post-deploy observation window.

---

## 5. Exit semantics in software work

### `done`

Use when:

1. expected software delta is achieved;
2. no immediate corrective or follow-up `CF` is needed;
3. `Result=+1`.

### `repeat`

Use when:

1. current flow is complete;
2. software work must continue immediately in a new `CF`;
3. the next issue has a sharpened delta.

Typical cases:

1. implementation succeeded but review opened a new required fix;
2. implementation revealed a second-order issue;
3. intended change was only partially achieved.

### `final`

Use when:

1. the chosen software change is inapplicable in current and expected target
   contours;
2. `Result=0`;
3. no near-term return is planned.

### `delayed`

Use when:

1. the change is currently inapplicable;
2. `Result=0`;
3. return depends on explicit future conditions.

Typical return conditions:

1. dependency release;
2. policy decision;
3. architecture change upstream;
4. resource window;
5. environment availability.

---

## 6. Domain notes

1. A pull request is not automatically equal to `implement`; it is evidence of
   `implement`.
2. Passing tests are not automatically equal to `done`; they are evidence for
   `evaluate`.
3. Rollback is modeled as a new forward `ChangeFlow`, not deletion of history.
4. `Result=0` must not be used as a synonym for "not implemented yet".

---

## 7. References

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
- [MMCF-DomainLexicon-Software-v1](../MMCF-DomainLexicon-Software-v1.md)
- [MMCF-Time-and-Cost-FROR-Alignment](../MMCF-Time-and-Cost-FROR-Alignment.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Experience](../../../fcdm-core/theory/cdm/Specifications/Experience/Experience-Canonical.md)
