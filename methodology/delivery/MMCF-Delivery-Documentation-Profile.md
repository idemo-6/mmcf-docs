---
title: "MMCF Delivery: Documentation Profile"
date: 2026-03-11
tags: [MMCF, delivery, documentation, profile, ChangeFlow]
status: profile-draft
---

# MMCF Delivery: Documentation Profile

## 1. Scope

This profile defines the terminal `ChangeFlow` interpretation for documentation
work that is not primarily scientific claim-validation work.

Typical targets:

1. product documentation;
2. engineering playbooks;
3. contributor guides;
4. operational runbooks;
5. API or architecture explanations.

Scientific/theoretical documentation is covered separately by:

- [MMCF-Delivery-Scientific-Documentation-Profile](./MMCF-Delivery-Scientific-Documentation-Profile.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)

---

## 2. Carrier and artifact

In this profile, the carrier entity is a document or document set.

A terminal issue still represents one `ChangeFlow` over that documentation
carrier:

- clarify;
- restructure;
- align terminology;
- repair contradictions;
- introduce missing guidance;
- remove obsolete guidance.

The issue is not equal to a single commit or one wording edit.

---

## 3. Phase interpretation

### `collect`

Minimum:

1. identify the document carrier;
2. identify the intended documentation delta;
3. capture audience and active context.

Typical evidence:

- missing section request;
- reader confusion report;
- stale content note;
- broken navigation/reference list;
- terminology mismatch.

### `analyze`

Minimum:

1. identify the current semantic/document structure;
2. locate contradictions, gaps, or ambiguity;
3. identify the governing canon/profile that constrains the document.

### `forecast`

Minimum:

1. produce admissible structure or wording paths;
2. compare clarity, compatibility, and maintenance costs;
3. identify likely review/acceptance path.

### `decide`

Minimum:

1. choose one document change strategy;
2. confirm applicability in the current context and policy;
3. freeze the target structure for this flow.

### `implement`

Documentation interpretation:

1. text, structure, links, or examples are actually updated;
2. the chosen narrative structure is materially applied;
3. the document carrier is changed, not only proposed.

Typical artifacts:

- markdown update;
- section move;
- glossary alignment;
- template update;
- link/reference fix.

### `evaluate`

Minimum:

1. verify that the intended documentation delta is reached;
2. verify clarity, consistency, and navigability;
3. record runtime result and next-step decision.

Typical evidence:

- review comment resolution;
- link/reference check;
- terminology consistency check;
- example correctness check;
- reader-oriented acceptance note.

---

## 4. Evaluate criteria

Recommended checks for documentation `evaluate`:

1. consistency with governing documents;
2. absence of contradiction in the updated scope;
3. sufficient clarity for intended audience;
4. working references and internal links;
5. no accidental semantic regressions in adjacent sections.

Possible runtime results:

1. `+1` if the intended clarification/alignment was achieved;
2. `-1` if the update introduced confusion, contradiction, or regression;
3. `0` if the intended change is not applicable in the current context.

---

## 5. Exit semantics

### `done`

Use when:

1. the intended documentation delta is reached;
2. no immediate next `CF` is needed;
3. `Result=+1`.

### `repeat`

Use when:

1. the current pass is complete;
2. another immediate pass is required for the same document contour;
3. the next issue has a sharper follow-up delta.

Typical cases:

1. wording fixed but examples still missing;
2. index fixed but cross-document links still need cleanup;
3. structure aligned but downstream documents now need synchronized updates.

### `final`

Use when:

1. the requested change is not applicable to the document contour;
2. `Result=0`;
3. no return is planned.

### `delayed`

Use when:

1. the requested documentation change is currently inapplicable;
2. `Result=0`;
3. return depends on a future event.

Typical return conditions:

1. upstream spec freeze;
2. terminology decision;
3. missing implementation details;
4. unresolved governance conflict.

---

## 6. Domain notes

1. editorial movement and semantic movement are not always the same; `evaluate`
   should check both.
2. "text was changed" is not sufficient evidence of success.
3. if the document is constrained by canon/profile documents, compatibility is
   part of `evaluate`, not an optional afterthought.
4. `Result=0` must not be used for "not enough time to write".

---

## 7. References

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
- [MMCF-Term-Legacy-Mapping](../MMCF-Term-Legacy-Mapping.md)
- [MMCF-CDM-Alignment-Matrix](../MMCF-CDM-Alignment-Matrix.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Experience](../../../fcdm-core/theory/cdm/Specifications/Experience/Experience-Canonical.md)
