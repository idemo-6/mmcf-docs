---
title: "MMCF: Conflict and Applicability Profile (CDM/FROR-aligned)"
date: 2026-03-05
tags: [MMCF, CDM, FROR, conflict, applicability, profile]
status: profile-draft
---

# MMCF: Conflict and Applicability Profile

## 1. Scope

Документ нормализует legacy-связку `рассинхрон/триггер` в согласованную
Delta-first схему MMCF/CDM.

Этот профиль не переопределяет `Intent`, `Context`, `ChangeFlow` или
runtime-семантику `Result`. Он фиксирует:

1. что первичным объектом является `Delta`;
2. что `conflict` является квалифицированным случаем `Delta`, а не ее
   универсальным синонимом;
3. как `Intent` и `Applicable` связаны с `Delta`;
4. как `trigger` должен быть сужен до policy-layer.

---

## 2. CDM-aligned core

### 2.1 Delta as primary object

`Delta` в MMCF трактуется как наблюдаемое структурное несоответствие между
актуальным и референсным состоянием в активном контексте.

Формально:

`Delta := diff(S_actual, S_ref | C_active)`.

Где `S_ref` может означать:

1. требуемое состояние;
2. допустимое состояние;
3. ожидаемое состояние;
4. предпочтительное состояние.

Следствие:

- `Delta` покрывает и деградацию, и opportunity-driven change;
- без `Delta` нет обоснованного `Intent`.

### 2.2 Conflict as qualification

`conflict` в MMCF не является универсальным именем любой `Delta`.

Полная допустимая taxonomy конфликтных классов задается в:
[MMCF-Conflict-Taxonomy-Canonical](./MMCF-Conflict-Taxonomy-Canonical.md).

Нормативно:

1. `conflict` — это квалифицированный случай или режим чтения `Delta`;
2. одна и та же `Delta` может быть квалифицирована как:
   - `state-conflict`,
   - `context-conflict`,
   - `constraint-conflict`,
   - `authority-conflict`,
   - `resource-conflict`,
   - `opportunity`,
   - `monitor-only`;
3. термин `conflict` не должен использоваться как общий синоним `Delta` в
   случаях, где смысл не конфликтный.

### 2.3 Intent derivation

`Intent` является нормализованной смысловой формой `Delta`:

`Intent := norm(Delta, C_active)`.

Следствие:

1. нет `Delta` -> нет обоснованного `Intent`;
2. одинаковый `Delta` может давать разные `Intent` в разных контекстах;
3. не каждая `Delta` обязана сразу materialize'иться в active `ChangeFlow`;
4. до promotion `Delta` может жить как `DeltaRecord` в `DeltaRegistry`.

### 2.4 Applicability gate

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

## 4. Trigger Policy Boundary

`trigger` в MMCF не является raw-фактом мира и не должен использоваться как
универсальный базовый объект change.

`trigger` допустим только как policy-signal.

Типовые допустимые trigger-signals:

1. `start_cf`
2. `start_meta_cf`
3. `force_collapse`
4. `escalate`
5. `defer`
6. `revalidate`

Это сохраняет legacy-интуицию о запуске изменения, но выносит `trigger` на
policy-layer, а не на уровень первичной онтологии.

---

## 5. FROR Alignment

### 5.1 Separation of layers

- CDM runtime-слой: `Applicable`, `Result in {+1,0,-1}`, phase-gates.
- MMCF pre-flow слой: `Delta`, `Qualification`, `DeltaRegistry`, `Promotion`.
- FROR аналитический слой: классы и стоимость фиксации (`tau`, `c_fix`).

### 5.2 No-cost transition rule

`FROR.0_no_cost_transition` не равен `CDM.Result=0`.

No-cost переход фиксируется как аналитическая метка при `Result in {+1,-1}`
или отдельный non-result флаг, но не как `ApplicabilityFailure`.

---

## 6. Operational Flow (MMCF)

Нормализованный маршрут для Delta-oriented change линии:

1. Зафиксировать `Event` и выделить из него `Delta`.
2. Создать `DeltaRecord` в `DeltaRegistry`.
3. Выполнить `Qualification/Triage`.
4. Если выбран `promote`, сформировать `Intent`.
5. После promotion materialize'ить terminal `ChangeFlow`.
6. Выполнить `Applicable`-gate в `decide`.
7. При `Applicable=true`: `CF5 -> CF6 -> Result in {+1,-1}`.
8. При `Applicable=false`: `CF4 -> CF6(inapplicable) -> Result=0`.

---

## 7. Legacy Term Normalization

1. `рассинхронизация` -> `Delta`.
2. `триггер` (универсальный драйвер) -> `trigger` только как policy-signal.
3. `конфликт` -> квалифицированный случай `Delta` или отдельный тип
   несовместимости контекстов/ограничений/authority.

Термин `trigger` допускается только в канонически заданных документах
политики и не заменяет `Delta` как базовый pre-flow объект.

---

## 8. Gateway and Responsibility Clarification

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

## 9. Minimal Examples

### 9.1 Applicable path

- `Delta` зафиксирован и при необходимости promoted в terminal `ChangeFlow`;
- `Intent` применим в `C_active` и текущей `LC_phase`;
- выполнен `CF5`;
- `CF6` фиксирует `Result=+1` или `Result=-1`.

### 9.2 Inapplicable path

- `Delta` зафиксирован и promoted в terminal `ChangeFlow`;
- выбранный `Intent` неприменим в текущем контексте;
- `CF5` не выполняется;
- `CF6` фиксирует `Result=0` как `ApplicabilityFailure`.

---

## 10. Anti-Patterns

Запрещено:

1. Называть любой `Result=0` "нулевой пользой" без проверки `Applicable`.
2. Смешивать FROR no-cost семантику с runtime-неприменимостью CDM.
3. Использовать `триггер` как универсальный канонический термин MMCF вне
   профилей trigger-policy.
4. Использовать `conflict` как обязательное имя любой `Delta`.
5. Считать, что любая зафиксированная `Delta` должна немедленно стать active
   `ChangeFlow`.

---

## 11. Acceptance Checklist

1. В тексте документа есть явная формула `Result=0 <=> ApplicabilityFailure`.
2. Есть разделение CDM runtime, MMCF pre-flow слоя и FROR аналитики.
3. `Delta` описан как первичный pre-flow объект в активном контексте.
4. `conflict` не подменяет универсально `Delta`.
5. Нет подмены `Delta` термином `trigger` как базового общего объекта.

---

## 12. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Conflict-Taxonomy-Canonical](./MMCF-Conflict-Taxonomy-Canonical.md)
- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](./MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)
- [MMCF-Term-Legacy-Mapping](./MMCF-Term-Legacy-Mapping.md)
- [MMCF-Applicability-Result-Alignment-Matrix](./MMCF-Applicability-Result-Alignment-Matrix.md)
- [CDM Intent Spec](../../fcdm-core/theory/cdm/Specifications/Intent-1.1.5.md)
- [CDM CtxL Canonical](../../fcdm-core/theory/cdm/Specifications/CtxL/CtxL-Canonical.md)
- [CDM CF-LC Evaluate](../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [CDM ChangeFlow-6 Canonical](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [FROR <-> CDM Bridge](../../fcdm-core/theory/cdm/bridge/FROR_CDM_bridge.md)
