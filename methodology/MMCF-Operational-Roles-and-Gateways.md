---
title: "MMCF: Operational Roles and Gateways Profile (CF1..CF6, CDM-aligned)"
date: 2026-03-05
tags: [MMCF, CDM, roles, gateways, phase-transition, profile]
status: profile-draft
---

# MMCF: Operational Roles and Gateways Profile

## 1. Scope

Документ задаёт прикладной профиль MMCF для:

1. ролевого представления фаз `CF1..CF6`;
2. многоуровневой ролевой рамки `Global > Domain > Local`;
3. контрактов фазовых переходов `LC-PT` и `CF-PT`;
4. разделения ответственности между фазой и переходом.

Профиль совместим с каноном CDM и не переопределяет `ChangeFlow-6`.
Нормативная база `LC-PT/CF-PT` берется из CDM `PhaseTransition_*`,
MMCF задает реализацию и role-mapping для управленческого контура.

---

## 2. Profile Principle

В операционном профиле MMCF:

- каждая фаза `CFi` представляется как отдельная роль-функция;
- переход `CFi => CF(i+1)` представляется как отдельный gateway-контракт;
- сбой gateway не считается автоматически сбоем обеих соседних фаз.

Контекст (`C_active/C_coord/C_meta`) задает рамку применимости и координации,
а не заменяет ролевую модель уровней и фаз.

---

## 3. Role Stack by Level

Роли задаются в иерархии:

`Global > Domain > Local`

Для каждого уровня `L` фиксируются минимум:

1. `LevelOwner(L)` — owner уровня (authority/policy owner).
2. `LCPhaseOwner(L, LCi)` — owner фаз `LC` в границах уровня.
3. `CFPhaseOwner(L, CFi)` — owner фаз `CF` в границах уровня.
4. `TransitionOwner(L, PT)` — owner фазового перехода (`LC-PT` или `CF-PT`).

Нормативно:

1. Роль трактуется как функция ответственности, а не как конкретный человек.
2. Один агент может совмещать несколько ролей, если сохраняется аудитируемость.
3. Для каждого `flow_id` должна быть трассировка owner-ролей по уровням.

---

## 4. Role-Phase Mapping (`CF1..CF6`)

| Canonical phase | Domain alias | Operational role | Minimal output |
|---|---|---|---|
| `CF1` | `collect` | `Collector` | контекстный вход и фиксация исходных ограничений |
| `CF2` | `analyze` | `Analyzer` | консистентная интерпретация контекста |
| `CF3` | `forecast` | `Forecaster` | множество допустимых траекторий |
| `CF4` | `decide` | `Decider` | выбранная траектория + rationale |
| `CF5` | `implement` | `Implementer` | физический commit изменения |
| `CF6` | `evaluate` | `Evaluator` | `Result`, обновление `Experience`, обновление метрик LC |

Нормативно:

1. Каноническая запись фаз остаётся `CF1..CF6`.
2. Алиасы `collect/analyze/...` используются только как подписи.

---

## 5. Phase Transition Taxonomy

### 5.1 `LC-PT` (LifeCycle transition)

`LC-PT` — переход между устойчивыми режимами функционирования системы:

1. убывание устойчивости старого режима;
2. достижение устойчивости нового режима.

До фиксации устойчивости нового режима ответственность несут минимум два owner:

1. `LCPhaseOwner(prev)`;
2. `LCPhaseOwner(next)`.

Критерий закрытия `LC-PT`:

- не только старт новой фазы,
- а подтвержденная устойчивость нового режима по policy-критериям уровня.

### 5.2 `CF-PT` (ChangeFlow transition)

`CF-PT` — операционный handoff между фазами `CFi => CF(i+1)`:

1. предшествующая фаза завершена;
2. следующая фаза еще не начала валидное исполнение.

Допустимы два режима:

1. `event-based transition` — событие завершения/принятия handoff.
2. `process-based transition` — отдельный процесс передачи (канал, очередь, email и т.д.).

В `event-based` переходе обычно участвуют 2 owner:

1. `CFPhaseOwner(from)`;
2. `CFPhaseOwner(to)`.

В `process-based` переходе обычно участвуют 3 owner:

1. `CFPhaseOwner(from)`;
2. `TransitionExecutor` (исполнитель передачи);
3. `CFPhaseOwner(to)`.

CDM-alignment:

1. `event-based/process-based` используются как operational labels;
2. каноническая CDM-классификация остается `безусловный/условный`;
3. `TransitionExecutor` — профильное имя канонической роли `transition_executor`;
4. уточнение о том, что незакрытый `CF-PT` может оставаться в `In-Transition`,
   а только финальный отрицательный исход прерывает `ChangeFlow`, берется из
   [CDM PhaseTransition-CF](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md).

---

## 6. Gateway Contract (`CF-PT`)

Для каждого перехода `CFi => CF(i+1)` gateway обязан фиксировать минимум:

1. `from_phase`, `to_phase`, `flow_id`;
2. `input_ref`, `output_ref`, `context_ref`;
3. `result in {true,false}`;
4. `t_start`, `t_end`;
5. `failure_reason` (если `result=false`).

Дополнительно для `CF4 => CF5`:

1. обязательная проверка `Applicable(Intent, C_active, LC_phase)`;
2. при `Applicable=false` выполняется `CF4 => CF6 (inapplicable)`.

---

## 7. Transition State Markers (`CF-PT`)

Для process-based handoff рекомендуется фиксировать этапы:

1. `phase_from_completed`;
2. `handoff_dispatched`;
3. `handoff_received`;
4. `handoff_accepted`;
5. `phase_to_ready`;
6. `transition_closed`.

Норматив:

1. завершение `phase_from` не означает автоматически закрытый переход;
2. отсутствие `handoff_received/accepted` блокирует валидный старт `phase_to`;
3. такой сбой классифицируется как `GatewayFailure`, а не как default failure фазы.

---

## 8. Responsibility Model

### 8.1 Phase responsibility

Владелец фазы отвечает за:

1. корректность логики внутри фазы;
2. корректность артефакта выхода фазы;
3. соблюдение входных предпосылок фазы.

### 8.2 Gateway responsibility

Владелец gateway отвечает за:

1. доставку и целостность передачи;
2. валидацию технической/процессной готовности следующей фазы;
3. фиксацию результата перехода и причины отказа.

### 8.3 LC transition responsibility

В `LC-PT` совместная ответственность `LCPhaseOwner(prev/next)` сохраняется
до валидации устойчивости нового режима.

---

## 9. Failure Taxonomy

Сбои классифицируются как:

1. `PhaseLogicFailure` — ошибка внутри фазы.
2. `ApplicabilityFailure` — неприменимость в активном контексте/фазе LC (`Result=0`).
3. `GatewayFailure` — срыв передачи/согласования между фазами.

Правило:

- `GatewayFailure` не смешивается с `ApplicabilityFailure`;
- `ApplicabilityFailure` не смешивается с `FROR.no_cost_transition`.
- в process-based `CF-PT` недоставка/непринятие handoff трактуется как `GatewayFailure`.

---

## 10. Delegation Rules

1. Полный цикл для классов систем `>=2`: `CF1..CF6`.
2. Для делегированной исполнительной проекции допускается `CF5-only`:
   upstream-контур делегирует `CF1..CF4` и `CF6`.
3. Перед локальным `CF5` обязателен валидный attest (если требует LC-политика).
4. Даже при делегировании gateway-контракты переходов остаются обязательными.

---

## 11. Minimal Operational Trace

Минимальный журнал для одного `flow_id`:

1. phase events: `CF1.Start/End` ... `CF6.Start/End`;
2. transition events: `CF.Transition(CFi, CFj, result, reason?)`;
3. applicability event для `CF4 => CF5`;
4. итоговый `Result in {+1,0,-1}` в `CF6`.

---

## 12. Anti-Patterns

Запрещено:

1. Писать "роль = человек" как базовое правило MMCF.
2. Считать любой провал `CFi => CF(i+1)` ошибкой обеих фаз.
3. Выполнять `CF5` без зафиксированной применимости.
4. Заменять каноническую фазовую нотацию только алиасами.
5. Смешивать `LC-PT` и `CF-PT` как один и тот же тип перехода.
6. Считать завершение `from`-фазы достаточным доказательством закрытого handoff.

---

## 13. Acceptance Checklist

1. В документе/процессе явно выделены роли уровня, роли фаз и роли переходов.
2. Разведены `LC-PT` и `CF-PT` с отдельными критериями закрытия.
3. Для переходов фиксируются `result` и `failure_reason`.
4. Для process-based handoff фиксируются состояния передачи (минимум received/accepted).
5. Для ветки inapplicable используется `CF4 => CF6`.
6. `Result=0` трактуется только как `ApplicabilityFailure`.
7. Используется каноническая нотация `CF1..CF6`.

---

## 14. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Term-Legacy-Mapping](./MMCF-Term-Legacy-Mapping.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Context-Canonical](./MMCF-Context-Canonical.md)
- [MMCF-Context-Coordination-and-Meta-Policy](./MMCF-Context-Coordination-and-Meta-Policy.md)
- [MMCF-Minimal-Working-Model](./MMCF-Minimal-Working-Model.md)
- [MMCF-CDM-Alignment-Matrix](./MMCF-CDM-Alignment-Matrix.md)
- [CDM ChangeFlow-6 Canonical](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM PhaseTransition-CF](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
- [CDM PhaseTransition Overview](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition_Overview.md)
- [CDM System Canonical](../../fcdm-core/theory/cdm/Specifications/System/System-Canonical.md)
