---
title: "MMCF: Time and Cost FROR Alignment Profile"
date: 2026-03-05
tags: [MMCF, CDM, FROR, time, cost, tau, alignment]
status: profile-draft
---

# MMCF: Time and Cost FROR Alignment Profile

## 1. Scope

Документ задаёт, как в MMCF интерпретировать время и стоимость изменений
через FROR-канон без конфликта с CDM runtime.

Профиль обязателен для всех текстов, где упоминаются:

- "время как редукции/шаги";
- энергетическая цена изменений;
- необратимость и rollback-семантика.

---

## 2. Layer Separation (mandatory)

### 2.1 CDM runtime layer

В CDM runtime фиксируются:

- фазовая логика `CF1..CF6`;
- `Applicable(Intent, C_active, LC_phase)`;
- `Result in {+1,0,-1}`.

Норматив:

`Result=0 <=> ApplicabilityFailure`.

### 2.2 FROR analytical layer

В FROR-слое фиксируются:

- `tau` — накопленная цена фиксации;
- `c_fix` — цена акта фиксации;
- `c_bg` / `D` — фоновая диссипация и её накопление;
- инварианты необратимости при конечном ресурсе.

---

## 3. Canonical Time Interpretation

В профиле MMCF:

1. Операционное внутреннее время процесса интерпретируется через `tau`:
   `tau_{t+1} = tau_t + c_fix(sigma_t, sigma_{t+1})`.
2. Внешнее календарное время (`t_cal`) не тождественно `tau`.
3. Рост `t_cal` без платных актов фиксации не означает рост `tau`.

---

## 4. Cost Accounting Policy

Нормативно разделяются два счётчика:

1. `tau` — только цена фиксации (`c_fix`).
2. `D` — фоновая диссипация (`c_bg`).

Следствия:

1. Возможен режим `c_bg > 0` при неизменном `tau`.
2. Нельзя вычислять `tau` как сумму любой процессной стоимости.
3. Для утверждений о необратимости требуется явная модель потери
   различимости/rollback-цены, а не только "прошло много времени".

---

## 5. Mapping to ChangeFlow

Рекомендуемое прикладное сопоставление:

1. `CF1..CF4`: подготовка выбора, может расти `D`; рост `tau` зависит от
   фактов фиксации в конкретной нормализации.
2. `CF5`: primary commit-point, где ожидается ненулевая цена фиксации.
3. `CF6`: семантическая фиксация результата (`Result`) и обновление LC-метрик.

Важно:

- `CF5` как commit-point не означает автоматической подмены FROR-метрик;
- FROR-метрики должны быть заданы явно и воспроизводимо.

---

## 6. Lambda/Reduction Terms Policy

1. Lambda- или редукционные термины допустимы только как профильная
   интерпретация, согласованная с FROR-документами.
2. Запрещено формулировать "физические" утверждения о времени/энергии в MMCF
   без ссылок на FROR-канон и bridge.
3. "k как время" допускается только как частный счётчик, если явно задано,
   как он отображается в `tau` и какие затраты относятся к `c_fix`.

---

## 7. Rollback Semantics

1. После commit rollback в канонической связке трактуется как новый
   согласованный переход (forward-compensation), а не стирание истории.
2. Коррекция выполняется новым ChangeFlow.
3. Утверждения об "абсолютной невозможности" rollback вне ресурсной модели
   считаются некорректными; канон фиксирует эффективную (ресурсную)
   необратимость.

---

## 8. Minimal Data Contract

Минимальные поля для time/cost-профиля:

1. `flow_id`
2. `normalization_id`
3. `c_fix_rule`
4. `c_bg_rule`
5. `tau_delta`
6. `D_delta`
7. `mapping_to_cf_phase`
8. `evidence_refs`

---

## 9. Anti-Patterns

Запрещено:

1. Смешивать `Result=0` и `FROR.no_cost_transition`.
2. Использовать `tau` как синоним календарного времени.
3. Складывать `c_fix` и `c_bg` без отдельного учёта.
4. Использовать legacy-термины (`ODS`, `OmniCycle`) в основном тексте.
5. Давать lambda-физику без FROR-референса.

---

## 10. Acceptance Checklist

1. В документе явно разведены CDM runtime и FROR analytical слои.
2. Определены правила расчёта `tau` и `D`.
3. Есть явная ссылка на `Result=0 <=> ApplicabilityFailure`.
4. Нет неканоничных утверждений о времени вне FROR-каркаса.
5. Указана семантика compensation вместо "исторического стирания".

---

## 11. Normative References

- [MMCF-Canonical](../core/MMCF-Canonical.md)
- [MMCF-Term-Legacy-Mapping](./MMCF-Term-Legacy-Mapping.md)
- [MMCF-Conflict-and-Applicability-Profile](../context/MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Applicability-Result-Alignment-Matrix](../core/MMCF-Applicability-Result-Alignment-Matrix.md)
- [CDM ChangeFlow-6 Canonical](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [FROR Main](../../../fcdm-core/theory/fror/FROR_main.md)
- [FROR Axioms](../../../fcdm-core/theory/fror/FROR_axioms_v0_2.md)
- [FROR Normalizations Invariants](../../../fcdm-core/theory/fror/FROR_normalizations_invariants.md)
- [FROR <-> CDM Bridge](../../../fcdm-core/theory/cdm/bridge/FROR_CDM_bridge.md)
