---
title: "MMCF: Claim Governance Applicability Profile"
date: 2026-03-12
tags: [MMCF, claim, governance, applicability, profile]
status: profile-draft
---

# MMCF: Claim Governance Applicability Profile

## 1. Scope

Этот документ определяет, когда claim-governance является обязательной,
опциональной или не требуется для рабочего объекта в MMCF.

Профиль не переопределяет саму шкалу `ClaimStatus`. Он задает только режим
применимости claim-discipline к разным классам work objects.

---

## 2. CDM foundation

Профиль опирается на следующие положения CDM/MMCF:

1. `ClaimStatus` является свойством утверждения, а не версии документа или
   runtime-status work unit.
2. Переходы `Conjecture -> Protocol -> Validated -> Core` остаются
   последовательными.
3. Изменение claim-status требует evidence и event trace.
4. `EntityVersion` и `ClaimStatus` остаются независимыми governance-осями.

---

## 3. Base Principle

Claim-governance применяется к claims, а не к объектам целиком.

Следовательно, MMCF различает не типы claims, а режимы обязательности
claim-discipline для данного класса work object:

1. `claim-required`
2. `claim-optional`
3. `claim-none`

---

## 4. Applicability Modes

### 4.1 `claim-required`

Используйте этот режим, когда объект содержательно включает или изменяет:

1. формальные claims и инварианты;
2. валидационные verdicts;
3. нормативные отображения и compatibility claims;
4. канонические или теоретические утверждения;
5. результаты, где claim-trace является частью самого смысла объекта.

В этом режиме затронутые claims должны оставаться трассируемыми через:

1. `claim_id`;
2. `claim_status_impact`;
3. `evidence_refs` или protocol refs;
4. явную evaluate-note о влиянии на claim-governance.

### 4.2 `claim-optional`

Используйте этот режим, когда объект не является в первую очередь реестром
claims, но может содержать ограниченное число сильных утверждений, гарантий или
совместимостных обещаний.

В этом режиме claim-discipline применяется только к явно размеченным claims.

### 4.3 `claim-none`

Используйте этот режим, когда объект прежде всего:

1. операционный;
2. объяснительный;
3. навигационный;
4. редакционный;
5. procedural, без выгоды от отдельного claim-trace.

В этом режиме claim-specific closure fields по умолчанию не требуются.

---

## 5. Generic Mapping Rules

Рекомендуемая классификация по умолчанию:

1. canonical/theoretical texts -> `claim-required`
2. alignment matrices and validation reports -> `claim-required`
3. specs or architecture texts with explicit guarantees -> `claim-optional`
4. general engineering or product documentation -> `claim-none`
5. guides, instructions, indexes, glossaries, examples -> `claim-none`

Эта классификация может уточняться доменным профилем, но смягчение режима
требует явного обоснования.

---

## 6. Impact On Work Unit Closure

### 6.1 For `claim-required`

Evaluate/closure должен явно surface как минимум:

1. `claim_ids_affected`
2. `claim_status_impact`
3. `protocol_or_evidence_refs`
4. `norm_or_canon_compatibility`, когда релевантно

### 6.2 For `claim-optional`

Claim-fields включаются только тогда, когда work unit затрагивает явно
отслеживаемый claim.

### 6.3 For `claim-none`

Closure не требует claim-specific fields, если только доменный профиль не
накладывает дополнительное правило.

---

## 7. Anti-Patterns

Запрещено или нежелательно:

1. трактовать каждое изменение объекта как изменение claim-status;
2. кодировать `ClaimStatus` в phase-status или terminal exit work unit;
3. использовать `claim-required` для purely navigational или procedural objects;
4. проводить claim-status changes без evidence trace;
5. смешивать claim-governance с versioning самой сущности.

---

## 8. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [Claim-Maturity-Canonical](./Claim-Maturity-Canonical.md)
- [Versioning-Canonical](./Versioning-Canonical.md)
- [CDM Claim Maturity Profile](../../fcdm-core/theory/cdm/Specifications/Claim-Maturity-Profile.md)
- [CDM Versioning Canonical](../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./delivery/MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
