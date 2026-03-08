---
title: "MMCF: CDM Alignment Matrix (Canonical vs Operational)"
date: 2026-03-06
tags: [MMCF, CDM, alignment, canonical, operational-profile]
status: working-draft
---

# MMCF: CDM Alignment Matrix

## 1. Назначение

Документ фиксирует границы между:

1. каноническим ядром CDM (source of truth);
2. профилем реализации MMCF;
3. допустимыми точками расширения без конфликта с каноном.

---

## 2. Принцип разграничения

1. Если сущность определяет общую онтологию/инварианты ChangeFlow и LifeCycle,
   она закрепляется в CDM.
2. Если сущность определяет прикладную организацию исполнения и governance,
   она закрепляется в MMCF.
3. MMCF не переопределяет канонические инварианты CDM.

---

## 3. Matrix: Canonical vs Profile

| Topic | CDM (canonical) | MMCF (operational profile) | Статус |
|---|---|---|---|
| Типы фазовых переходов | `LC-PT`, `CF-PT` | использует без переопределения | aligned |
| Семантика `LC-PT` | переход между режимами, критерий устойчивости | доменная операционализация критерия устойчивости | aligned |
| Семантика `CF-PT` | автономный handoff-процесс между фазами | gateway-контракт и трассировка handoff | aligned |
| Классификация PT | `безусловный/условный` | operational labels `event-based/process-based` | mapped |
| Участники `CF-PT` | `from_phase_owner`, `to_phase_owner`, optional `transition_executor` | `CFPhaseOwner(from/to)`, `TransitionExecutor` | mapped |
| Состояния `CF-PT` | минимальные markers `from_completed..transition_closed` | расширенный журнал и контрольные события | mapped |
| Поведение при `CF-PT result=false` | прерывание текущего `CF`, переход в `CF6` | фиксируется как `GatewayFailure` + завершение через `CF6` | aligned |
| Ролевые уровни исполнения | не задает орг-иерархию реализации | `Global > Domain > Local`, level/phase/transition owners | profile-owned |
| Контекстная координация | каноника Context/Coordination в CDM | `C_coord/C_meta` policy и escalation в управленческом контуре | profile-owned |

---

## 4. Invariants (must hold)

1. `LC-PT` и `CF-PT` не смешиваются как один тип перехода.
2. Завершение исходной фазы не означает автоматически закрытый `CF-PT`.
3. Для `CF-PT result=false` текущий `ChangeFlow` завершается через `CF6`.
4. `Result=0` в MMCF трактуется только как `ApplicabilityFailure` и не подменяет
   семантику `CF-PT result=false`.
5. Любое профильное расширение ролей в MMCF остается отображаемым в канонические
   роли перехода CDM.

---

## 5. Extension Points (allowed)

Допускается расширять в MMCF:

1. naming ролей (`Collector`, `Decider`, `TransitionExecutor` и т.д.);
2. ownership-модель уровня (`Global/Domain/Local`);
3. детализацию transition state machine и SLA/timeout;
4. governance-политики (`override`, `escalation`, `revalidation`);
5. domain-specific evidence/metrics.

Запрещено расширять в MMCF с переопределением:

1. базовой типологии PT (`LC-PT`, `CF-PT`);
2. канонической классификации `безусловный/условный`;
3. канонического поведения `CF-PT` при отказе.

---

## 6. Change Control Rule

При изменениях PT-семантики:

1. сначала обновляется CDM-канон (`PhaseTransition_*`),
2. затем обновляются профильные документы MMCF,
3. в MMCF фиксируется explicit mapping на соответствующие разделы CDM.

---

## 7. References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](./MMCF-Minimal-Working-Model.md)
- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Applicability-Result-Alignment-Matrix](./MMCF-Applicability-Result-Alignment-Matrix.md)
- [CDM PhaseTransition Overview](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition_Overview.md)
- [CDM PhaseTransition-LC](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-LC.md)
- [CDM PhaseTransition-CF](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
