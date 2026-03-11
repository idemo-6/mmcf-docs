---
title: "MMCF Delivery: Scientific Documentation Profile"
date: 2026-03-11
tags: [MMCF, delivery, scientific-documentation, claims, profile]
status: profile-draft
---

# MMCF Delivery: Scientific Documentation Profile

## 1. Scope

This profile defines the delivery interpretation of terminal `ChangeFlow`
issues for scientific, theoretical, and methodology-heavy documentation.

Typical targets:

1. canonical drafts;
2. theory profiles;
3. validation and experiment reports;
4. methodology alignment matrices;
5. claim-structured research notes.

This profile extends the general documentation profile with:

1. `ClaimStatus`;
2. evidence discipline;
3. canon-compatibility checks;
4. stronger `evaluate` requirements.

Claim applicability mode for this profile is `claim-required`, as defined in:

- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)

---

## 2. Carrier and governance axes

In this profile, the carrier entity is usually:

1. a theory document;
2. a methodology/profile document;
3. a validation package;
4. a claim-structured documentation set.

Two governance axes must remain separate:

1. `EntityVersion` = process state of the document carrier;
2. `ClaimStatus` = maturity of specific claims.

The terminal issue tracks one `ChangeFlow`.
It must not encode `ClaimStatus` in issue status.

---

## 3. Typical task classes

Typical terminal issue classes:

1. formalize a claim or invariant;
2. rewrite a section for canonical compatibility;
3. attach a protocol to an existing claim;
4. attach validation evidence;
5. prepare a candidate claim for canon acceptance;
6. reconcile conflicting formulations across documents.

---

## 4. Phase interpretation

### `collect`

Minimum:

1. identify the document carrier;
2. identify the target claim/document delta;
3. capture the relevant canon/profile context.

Typical evidence:

- conflicting formulations;
- missing protocol;
- stale claim status;
- unresolved reviewer objection;
- missing validation package.

### `analyze`

Minimum:

1. locate current claim/document state;
2. identify governing references and compatibility constraints;
3. classify whether the target delta is semantic, evidential, structural, or
   governance-related.

### `forecast`

Minimum:

1. produce admissible rewrite/evidence paths;
2. compare risk of semantic drift;
3. identify expected claim-status impact.

### `decide`

Minimum:

1. choose one path;
2. confirm applicability in current canon/profile context;
3. freeze the target claim/document move for this flow.

### `implement`

Scientific documentation interpretation:

1. the document text, evidence registry, or linked protocol/report is actually
   updated;
2. the chosen semantic structure is materially applied;
3. the claim/document contour has a real committed delta.

### `evaluate`

Minimum:

1. determine runtime result;
2. verify semantic compatibility with governing documents;
3. verify evidence sufficiency for the intended move;
4. record whether `ClaimStatus` implications exist.

---

## 5. Evaluate requirements

Recommended `evaluate` checklist:

1. compatibility with canon/profile references;
2. explicit evidence refs for all changed claims in scope;
3. no hidden skip-level move in `ClaimStatus`;
4. no mixing of `EntityVersion` and `ClaimStatus`;
5. explicit note whether a claim-status update is proposed, executed elsewhere,
   or not affected.

Recommended additional fields in the `CF6` summary for this profile:

1. `claim_ids_affected`
2. `claim_status_impact`
3. `canon_compatibility`
4. `protocol_or_evidence_refs`

---

## 6. Exit semantics

### `done`

Use when:

1. the intended scientific-documentation delta is achieved;
2. semantic and governance checks passed;
3. `Result=+1`;
4. no immediate follow-up `CF` is required.

### `repeat`

Use when:

1. the current flow is complete;
2. the next immediate pass is already clear;
3. the follow-up flow has a more specific delta.

Typical cases:

1. section rewritten, but protocol attachment still missing;
2. protocol attached, but validation evidence still needs its own pass;
3. alignment fixed in one document, but linked documents still require
   synchronization.

### `final`

Use when:

1. the requested scientific-documentation move is not applicable in the current
   canon/profile context;
2. `Result=0`;
3. no planned return exists.

### `delayed`

Use when:

1. the move is currently inapplicable;
2. `Result=0`;
3. return depends on future evidence, protocol, policy, or reviewer resolution.

Typical return conditions:

1. experiment completion;
2. protocol finalization;
3. claim registry decision;
4. canon update upstream;
5. cross-document conflict resolution.

---

## 7. Domain notes

1. A document merge is not the same as claim validation.
2. A successful text rewrite may still require `repeat` if the evidential layer
   is incomplete.
3. `ClaimStatus` updates should be traceable, but they remain a separate axis
   from terminal issue status.
4. `Result=0` means inapplicability, not "insufficiently proven".

---

## 8. References

- [MMCF-Delivery-Documentation-Profile](./MMCF-Delivery-Documentation-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
- [Claim-Maturity-Canonical](../Claim-Maturity-Canonical.md)
- [Versioning-Canonical](../Versioning-Canonical.md)
- [MMCF-CDM-Alignment-Matrix](../MMCF-CDM-Alignment-Matrix.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Experience](../../../fcdm-core/theory/cdm/Specifications/Experience/Experience-Canonical.md)
