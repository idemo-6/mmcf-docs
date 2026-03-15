---
title: "MMCF: Applicability and Result Alignment Matrix (CDM Runtime vs FROR Analytics)"
date: 2026-03-06
tags: [MMCF, CDM, FROR, applicability, result, alignment]
status: working-draft
---

# MMCF: Applicability and Result Alignment Matrix

## 1. Назначение

Документ фиксирует согласование между:

1. runtime-семантикой CDM (`Applicable`, `Result`, `CF6`),
2. аналитическим слоем FROR (`tau`, `c_fix`, `c_bg`, `D`),
3. прикладным профилем MMCF.

Цель: исключить смешение `Result=0`, `CF-PT result=false` и FROR no-cost.

---

## 2. Принцип разграничения

1. CDM определяет runtime-результаты ChangeFlow.
2. FROR определяет аналитическую стоимость/время фиксации.
3. MMCF связывает оба слоя без переопределения канона.

---

## 3. Matrix: Semantics and Mapping

| Case | CDM runtime meaning | FROR analytical meaning | MMCF interpretation | Status |
|---|---|---|---|---|
| `Applicable=false` в `CF4` | ветка `CF4 => CF6(inapplicable)` | не задает автоматически no-cost | `Result=0` как `ApplicabilityFailure` | aligned |
| `Result=0` | только `ApplicabilityFailure(Intent, C_active, LC_phase)` | не эквивалентен FROR-классу | используется только для неприменимости | aligned |
| `CF-PT attempt failure` | еще не runtime result flow | может добавлять время/стоимость без terminal outcome | retryable gateway event внутри того же PT | mapped |
| terminal `CF-PT result=false` | прерывание текущего `CF`, переход в `CF6` | может иметь любую стоимость по FROR-модели | классифицируется как transition/gateway failure | aligned |
| `FROR.no_cost_transition` | не является runtime result | аналитическая метка затрат (`tau_delta=0` в выбранной нормализации) | хранится как аналитический флаг/класс, не как `Result=0` | aligned |
| `Result in {+1,-1}` | применимое исполнение, успех/неуспех | стоимость/диссипация учитываются отдельно | результат и аналитика фиксируются раздельно | aligned |

---

## 4. Invariants (must hold)

1. `Result=0 <=> ApplicabilityFailure`.
2. terminal `CF-PT result=false` не кодируется как `Result=0` автоматически.
3. `FROR.no_cost_transition` не кодируется как `Result=0`.
4. Только terminal отказ перехода завершается через `CF6` по канону CDM.
5. Runtime-поля результата и аналитические FROR-поля ведутся раздельно.
6. attempt-level failures внутри retry policy не являются runtime result flow.

---

## 5. Minimal Data Contract

Для записи результата и аналитики в одном trace рекомендуется минимум:

1. `flow_id`
2. `cf_phase`
3. `applicable` (`true/false`)
4. `runtime_result` (`+1/0/-1`)
5. `runtime_failure_class` (`ApplicabilityFailure` / `PhaseLogicFailure` / `TransitionFailure`)
6. `pt_result` (`true/false`, если есть переход)
7. `pt_failure_reason` (если `pt_result=false`)
8. `pt_attempt_no`, если были retry attempts
9. `pt_failure_policy`, если используется retry policy
10. `fror_class` (например, `no_cost_transition`)
11. `tau_delta`
12. `D_delta`
13. `evidence_refs`

---

## 6. Anti-Patterns

Запрещено:

1. Интерпретировать любой `Result=0` как "нулевой эффект".
2. Подменять terminal `CF-PT result=false` кодом `Result=0` без проверки применимости.
3. Смешивать FROR no-cost и CDM runtime неприменимость в одном поле.
4. Делать вывод о no-cost только по календарному времени.
5. Считать `CF5` commit-point достаточным для вывода о величине `tau` без модели `c_fix`.
6. Считать каждую неуспешную попытку transition автоматически terminal failure потока.

---

## 7. Change Control Rule

При изменениях семантики `Applicable/Result`:

1. сначала обновляются канонические спецификации CDM/Bridge;
2. затем обновляются профильные документы MMCF;
3. mapping-правила этой матрицы синхронизируются в том же change-set.

---

## 8. References

- [MMCF-Conflict-and-Applicability-Profile](../context/MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Time-and-Cost-FROR-Alignment](../governance/MMCF-Time-and-Cost-FROR-Alignment.md)
- [MMCF-CDM-Alignment-Matrix](./MMCF-CDM-Alignment-Matrix.md)
- [CDM ChangeFlow-6 Canonical](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [CDM PhaseTransition-CF](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
- [FROR <-> CDM Bridge](../../../fcdm-core/theory/cdm/bridge/FROR_CDM_bridge.md)
- [FROR Main](../../../fcdm-core/theory/fror/FROR_main.md)
