---
title: "MMCF: Conflict and Applicability Profile (CDM/FROR-aligned)"
date: 2026-03-05
tags: [MMCF, CDM, FROR, conflict, applicability, profile]
status: profile-draft
---

# MMCF: Conflict and Applicability Profile

## 1. Scope

Документ нормализует legacy-связку `рассинхрон/триггер` в согласованную схему
MMCF/CDM:

`конфликт (Delta) -> Intent -> Applicable? -> Result`.

Профиль задаёт операционные правила для методологических текстов MMCF и не
переопределяет базовые спецификации CDM/FROR.

---

## 2. CDM-aligned core

### 2.1 Conflict as Delta

`конфликт` в MMCF трактуется как наблюдаемое структурное несоответствие `Delta`
между актуальным и требуемым/допустимым состоянием в активном контексте.

Формально:

`Delta := diff(S_actual, S_required | C_active)`.

### 2.2 Intent derivation

`Intent` является нормализованной смысловой формой `Delta`:

`Intent := norm(Delta, C_active)`.

Следствие:

- нет `Delta` -> нет обоснованного `Intent`;
- одинаковый `Delta` может давать разные `Intent` в разных контекстах.

### 2.3 Applicability gate

Реализация `Intent` в ChangeFlow допустима только при:

`Applicable(Intent, C_active, LC_phase) = true`.

При `Applicable=false` выполняется ветка `PT(CF4->CF6:inapplicable)`.

---

## 3. Runtime Semantics

Нормативно:

1. `Result=0 <=> ApplicabilityFailure(Intent, C_active, LC_phase)`.
2. Если `Applicable=true`, тогда `Result in {+1,-1}`.
3. `Result=0` не используется как "нейтральный успех" или "нулевой эффект".

---

## 4. FROR Alignment

### 4.1 Separation of layers

- CDM runtime-слой: `Result in {+1,0,-1}` и phase-gates.
- FROR аналитический слой: классы и стоимость фиксации (`tau`, `c_fix`).

### 4.2 No-cost transition rule

`FROR.0_no_cost_transition` не равен `CDM.Result=0`.

No-cost переход фиксируется как аналитическая метка при `Result in {+1,-1}`
или отдельный non-result флаг, но не как `ApplicabilityFailure`.

---

## 5. Legacy Term Normalization

1. `рассинхронизация` -> `конфликт (Delta)`.
2. `триггер` (универсальный драйвер) -> `конфликт/Delta` как базовый объект.
3. Термин `trigger` допускается только в канонически заданных документах
   политики (например, `MetaChangeFlow-Trigger-Policy`) и не заменяет
   общий термин `конфликт` в MMCF.

---

## 6. Operational Flow (MMCF)

Нормализованный маршрут для конфликт-ориентированной задачи:

1. Зафиксировать `Delta` в `CF1/CF2`.
2. Сформировать `Intent` и варианты траекторий (`CF3`).
3. Выполнить выбор (`CF4`) и проверить `Applicable`.
4. При `Applicable=true`: `CF5 -> CF6 -> Result in {+1,-1}`.
5. При `Applicable=false`: `CF4 -> CF6(inapplicable) -> Result=0`.

---

## 7. Gateway and Responsibility Clarification

Профиль фиксирует разделение ответственности:

1. Фаза отвечает за корректность своей логики.
2. Переход между фазами (`PhaseTransition/Gateway`) отвечает за доставку,
   связность и валидность передачи.
3. Сбой в gateway не является доказательством ошибки обеих фаз.

Это позволяет классифицировать сбои как:

- phase-logic failure;
- input/applicability failure;
- gateway/transition failure.

---

## 8. Minimal Examples

### 8.1 Applicable path

- `Delta` зафиксирован;
- `Intent` применим в `C_active` и текущей `LC_phase`;
- выполнен `CF5`;
- `CF6` фиксирует `Result=+1` или `Result=-1`.

### 8.2 Inapplicable path

- `Delta` зафиксирован;
- выбранный `Intent` неприменим в текущем контексте;
- `CF5` не выполняется;
- `CF6` фиксирует `Result=0` как `ApplicabilityFailure`.

---

## 9. Anti-Patterns

Запрещено:

1. Называть любой `Result=0` "нулевой пользой" без проверки `Applicable`.
2. Смешивать FROR no-cost семантику с runtime-неприменимостью CDM.
3. Использовать `триггер` как универсальный канонический термин MMCF вне
   профилей trigger-policy.

---

## 10. Acceptance Checklist

1. В тексте документа есть явная формула `Result=0 <=> ApplicabilityFailure`.
2. Есть разделение CDM runtime и FROR аналитики.
3. Конфликт описан как `Delta` в активном контексте.
4. Нет подмены `конфликта` термином `триггер` как базового общего объекта.

---

## 11. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Term-Legacy-Mapping](./MMCF-Term-Legacy-Mapping.md)
- [MMCF-Applicability-Result-Alignment-Matrix](./MMCF-Applicability-Result-Alignment-Matrix.md)
- [CDM Intent Spec](../../fcdm-core/theory/cdm/Specifications/Intent-1.1.5.md)
- [CDM CtxL Canonical](../../fcdm-core/theory/cdm/Specifications/CtxL/CtxL-Canonical.md)
- [CDM CF-LC Evaluate](../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [CDM ChangeFlow-6 Canonical](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [FROR <-> CDM Bridge](../../fcdm-core/theory/cdm/bridge/FROR_CDM_bridge.md)
