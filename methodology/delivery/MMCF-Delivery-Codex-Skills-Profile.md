---
title: "MMCF Delivery: Codex Skills Profile"
date: 2026-03-11
tags: [MMCF, delivery, Codex, skills, Linear]
status: working-draft
---

# MMCF Delivery: Codex Skills Profile

## 1. Purpose

This document defines the first project-specific Codex skill package for
operating MMCF-compatible terminal `ChangeFlow` issues in Linear.

The skills are operational helpers. They do not redefine the delivery profile.

---

## 2. Skill package v1

The initial package contains one entry skill plus four narrow skills:

1. `linear-mmcf`
   Entry skill and router for MMCF-compatible Linear work.
2. `linear-cf-intake`
   Create a new terminal `ChangeFlow` issue under an existing epic.
3. `linear-cf-advance`
   Advance a terminal issue to the next valid phase with `PT` awareness.
4. `linear-cf-close`
   Write the `CF6` summary and close the current flow with a valid terminal
   exit.
5. `linear-cf-repeat`
   Create the next sibling `ChangeFlow` issue from an evaluated source flow.

This split keeps one strong trigger surface while preserving narrow operational
skills underneath.

---

## 3. Scope of each skill

### `linear-mmcf`

Use when:

1. the user is talking about Linear through MMCF/CDM concepts rather than only
   generic ticket management;
2. the task may involve terminal `ChangeFlow`, `PhaseTransition`, `CF6`,
   `repeat`, or claim-aware delivery;
3. the exact narrow skill is not yet clear at the start of the turn.

### `linear-cf-intake`

Use when:

1. a new terminal issue must be created under an existing epic;
2. the issue must follow the naming convention, body template, and label rules;
3. `Artifact Type`, `Claim Mode`, and expected non-trivial `PT` transitions are
   known or can be inferred.

### `linear-cf-advance`

Use when:

1. an existing terminal issue must move to the next phase;
2. phase order and `Applicable` rules must be enforced;
3. approval, claim, or gateway bottlenecks must be reflected in comments and
   labels.

### `linear-cf-close`

Use when:

1. the issue is in `evaluate`;
2. `CF6` must be written with `Result`, `Failure class`, and `PT` summary;
3. the flow must close as `done`, `final`, or `delayed`, or be prepared for
   `repeat`.

### `linear-cf-repeat`

Use when:

1. the evaluated flow requires an immediate next `ChangeFlow`;
2. the next issue must inherit the same contour and base objective;
3. the title, body, links, and `CF6` references must be carried forward
   correctly.

---

## 4. Shared operating rules

All skills in this package must:

1. follow the delivery docs in `methodology/delivery/`;
2. treat terminal issue status as the phase layer only;
3. keep `PhaseTransition` trace separate from phase status;
4. not encode phase, exit, or active bottleneck in the issue title;
5. use labels only as issue-level visibility aids;
6. respect the rule `one terminal issue = one ChangeFlow`.

If Linear MCP cannot set a required custom field directly, the skill should:

1. still create or update the issue body and comments correctly;
2. mention the missing field update explicitly in the final summary.

---

## 5. Source location

The versioned skill sources for this profile live in:

- `mmcf-docs/codex-skills/`

The install target for live Codex usage is:

- `$HOME/.codex/skills/`

---

## 6. References

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
