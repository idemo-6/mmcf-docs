---
title: "MMCF Delivery: Claim Maturity Applicability Profile"
date: 2026-03-11
tags: [MMCF, delivery, claim-maturity, profile, documentation]
status: profile-draft
---

# MMCF Delivery: Claim Maturity Applicability Profile

## 1. Scope

This profile defines when `Claim Maturity` is required, optional, or not needed
for delivery documents and delivery-bound work items.

The profile is intended to prevent two opposite errors:

1. losing claim governance where it is methodologically required;
2. overloading ordinary operational documentation with unnecessary claim
   bureaucracy.

---

## 2. Core principle

`Claim Maturity` applies to claims, not to documents as a whole.

However, document classes differ in whether claim-bearing structure is:

1. mandatory;
2. optional;
3. normally absent.

Therefore this profile uses three applicability modes:

1. `claim-required`
2. `claim-optional`
3. `claim-none`

---

## 3. Applicability modes

### 3.1 `claim-required`

Use this mode when the document materially contains:

1. formal claims;
2. invariants;
3. methodological compatibility assertions;
4. validation verdicts;
5. canon-adjacent or theory-adjacent normative statements.

In this mode, affected claims must be traceable through:

1. `ClaimId`
2. `ClaimStatus`
3. `EvidenceRefs`
4. explicit `evaluate` note on claim impact

### 3.2 `claim-optional`

Use this mode when the document is not primarily a claim registry, but may
contain a limited set of strong assertions that benefit from claim discipline.

Typical examples:

1. architecture guarantees;
2. compatibility guarantees;
3. security assumptions;
4. performance assertions;
5. migration guarantees.

In this mode, only explicitly marked claims require claim tracking.

### 3.3 `claim-none`

Use this mode when the document is primarily operational, explanatory,
editorial, or navigational and does not benefit from claim governance.

Typical examples:

1. how-to guides;
2. runbooks;
3. examples;
4. navigation pages;
5. general product documentation.

In this mode, `ClaimStatus` is not required in terminal issue closure.

---

## 4. Recommended classification by document class

| Document class | Claim mode | Rule |
|---|---|---|
| scientific/theoretical documentation | `claim-required` | default |
| canonical documents | `claim-required` | default for key claims in scope |
| alignment matrices | `claim-required` | default for normative mappings and invariants |
| validation reports and protocols | `claim-required` | default |
| software architecture/spec documents with guarantees | `claim-optional` | use only for explicit strong assertions |
| general engineering documentation | `claim-none` | default |
| product/user documentation | `claim-none` | default |
| examples, indexes, glossaries | `claim-none` | default |

---

## 5. Impact on terminal `ChangeFlow`

### 5.1 For `claim-required`

The `CF6` summary should include:

1. `claim_ids_affected`
2. `claim_status_impact`
3. `protocol_or_evidence_refs`
4. `canon_compatibility`

### 5.2 For `claim-optional`

The `CF6` summary includes claim fields only when the issue explicitly touches a
tracked claim.

### 5.3 For `claim-none`

The `CF6` summary does not require claim-specific fields.

---

## 6. Domain mapping for current delivery profiles

Recommended defaults:

1. `MMCF Delivery: Scientific Documentation Profile` -> `claim-required`
2. `MMCF Delivery: Documentation Profile` -> `claim-none`
3. `MMCF Delivery: Software Profile` -> `claim-none` by default, `claim-optional`
   for architecture/spec/security/performance guarantee work
4. `MMCF Delivery: Linear Profile` -> `claim-none`
5. `MMCF Delivery: Terminal ChangeFlow Contract` -> `claim-none`

---

## 7. Anti-patterns

Forbidden or discouraged:

1. treating every document change as a claim-status change;
2. encoding `ClaimStatus` into terminal issue status;
3. using `claim-required` for examples, runbooks, indexes, or ordinary guides;
4. performing silent claim-status transitions without evidence trace.

---

## 8. References

- [Claim-Maturity-Canonical](../Claim-Maturity-Canonical.md)
- [Versioning-Canonical](../Versioning-Canonical.md)
- [MMCF-Delivery-Documentation-Profile](./MMCF-Delivery-Documentation-Profile.md)
- [MMCF-Delivery-Scientific-Documentation-Profile](./MMCF-Delivery-Scientific-Documentation-Profile.md)
- [MMCF-Delivery-Software-Profile](./MMCF-Delivery-Software-Profile.md)
