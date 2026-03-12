---
title: "MMCF: Minimal Working Model"
date: 2026-03-05
tags: [MMCF, CDM, minimal-model, context, governance, operations]
status: working-draft
---

# MMCF: Minimal Working Model

## 1. Scope

Документ фиксирует минимальную рабочую модель MMCF по итогам обсуждений:

1. как организована система;
2. как работают контексты;
3. как исполняется распределенный `CF1..CF6`;
4. как принимаются и эскалируются решения.

---

## 2. Базовый принцип

MMCF используется как прикладной контур управления недетерминированными
изменениями.

Используемый в MMCF канонический каркас CDM:

`CF = [CF1 => CF2 => CF3 => CF4 => CF5 => CF6]`

Доменные названия допустимы, но каноническая трассировка сохраняется.

---

## 3. System-of-Systems модель

1. `Y_parent` — родительская система (компания/группа).
2. `Y_child[i]` — дочерние системы (направления/подразделения/контуры).
3. Каждый узел имеет:
   - `LC_Y`,
   - `CF_Y`,
   - `C-Layer(Y)`,
   - `Policy_Y`,
   - `Authority_Y`.

Политики уровней:

1. `GlobalPolicy` (`Y_parent`)
2. `DomainPolicy` (`Y_child`)
3. `LocalPolicy` (команды)

Приоритет:

`Global > Domain > Local`

Минимум ролевого ownership для каждого уровня:

1. `LevelOwner(L)` — owner уровня (`L in {Global, Domain, Local}`).
2. `LCPhaseOwner(L, LCi)` — owner фаз `LC` на уровне `L`.
3. `CFPhaseOwner(L, CFi)` — owner фаз `CF` на уровне `L`.
4. `TransitionOwner(L, PT)` — owner фазовых переходов (`LC-PT`/`CF-PT`) на уровне `L`.

---

## 4. Контекстная модель

Контекст определяется как система ограничений и правил принятия решений,
а не как список отделов.

Минимальные объекты:

1. `C_candidates`
2. `C_active^k`
3. `C_coord`
4. `C_meta`

Правило:

- в ветке активен один `C_active^k`;
- мультиконтекстность реализуется через ветки + координацию.

---

## 5. Независимые и составные контексты

Независимые контексты:

1. разные KPI успеха;
2. разные hard-ограничения;
3. разные owners решения;
4. разная применимость одной траектории.

Составной контекст допустим, если одновременно:

1. согласован единый KPI;
2. сняты конфликтующие ограничения;
3. есть единый owner;
4. возможен единый `Applicable`-gate.

---

## 6. Распределенный ChangeFlow

Типовой жизненный маршрут:

1. `CF1` — инициатор/входной контур.
2. `CF2` — аналитический контур.
3. `CF3` — прогнозный контур.
4. `CF4` — центр принятия решения.
5. `CF5` — локальный исполнитель (место реального commit).
6. `CF6` — финальная оценка (локально/доменно/родительски).

Требования:

1. перед `CF5` обязателен `Applicable=true`;
2. handoff-контракт обязателен для каждого перехода;
3. для flow фиксируется один официальный `CF6` (final result boundary).

Типы фазовых переходов:

1. `LC-PT` — переход между устойчивыми режимами (`LCi => LC(i+1)`),
   закрывается по критерию устойчивости нового режима.
2. `CF-PT` — операционный handoff (`CFi => CF(i+1)`), бывает:
   - `inline`,
   - `process-based` (с отдельным исполнителем передачи).

Для распределённого `CF` действует operational default:

1. если соседние фазы реально исполняются разными агентами или контурами,
   соответствующий `CF-PT` следует считать `process-based` по умолчанию;
2. `inline` по умолчанию уместен прежде всего для локального или
   неразвернутого исполнения, где handoff не образует самостоятельного
   операционного контура;
3. это не означает, что каждый distributed `CF-PT` обязан materialize'иться
   в отдельный tool object, но по смыслу он остается process-based.

В process-based `CF-PT` недоставка/непринятие handoff классифицируется как
`GatewayFailure`, а не как default ошибка обеих фаз.

Нормативная семантика `LC-PT/CF-PT` наследуется из CDM
`PhaseTransition-LC/PhaseTransition-CF`; текущий документ задает
минимальный operational-профиль применения.

Для `CF-PT` см. также уточнение в
[CDM PhaseTransition-CF](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md):
незакрытый переход может оставаться в `In-Transition`, а `CF6` инициируется
только после финального `result=false`.

---

## 7. Applicability и Result

Норматив:

1. `Result=0 <=> ApplicabilityFailure(Intent, C_active, LC_phase)`.
2. при `Applicable=true` результат только `+1` или `-1`.
3. `Result=0` не трактуется как "нейтральный успех".

---

## 8. C_coord/C_meta policy (минимум)

### C_coord

1. иерархическая цепочка решений;
2. capability-override по risk/evidence/verification;
3. escalation timeout и escalation levels;
4. primary/fallback authority agents.

### C_meta

Обучение policy на сигналах:

1. `Result=0`;
2. gateway failures;
3. rework/compensation;
4. incidents/near-miss;
5. stable success patterns.

---

## 9. Evaluate modes

Для практики фиксируются три режима:

1. `delegated-evaluate` — final `CF6` в дочернем контуре.
2. `centralized-evaluate` — final `CF6` в родительском/координационном контуре.
3. `hybrid-evaluate` — локальный pre-eval + родительский final eval.

---

## 10. Priority model (minimum)

Обязательные классы приоритета:

1. `Delta`-источник изменения;
2. выбор исполнителя;
3. центр разрешения конфликтов реализации;
4. risk/regulatory hard-stop;
5. blast radius;
6. confidence/evidence;
7. cost-of-delay vs cost-of-change;
8. dependency critical-path;
9. authority mode (`auto/hybrid/manual`).

---

## 11. Когда эскалировать на уровень Y_parent

Эскалация обязательна, если:

1. затронуто 2+ дочерних системы;
2. конфликт с `GlobalPolicy` или hard-stop;
3. blast radius cross-system/critical;
4. истек timeout локального уровня;
5. локальный authority не принял валидное решение.

---

## 12. Минимум для запуска пилота

1. DomainLexicon для команды.
2. Context catalog (`C_candidates`, rules, owners).
3. Handoff logging по фазовым переходам.
4. Policy profile (`C_coord/C_meta`) с escalation и override.
5. Weekly KPI snapshots по `CF`, gateway и applicability.

---

## 13. Итог

MMCF в минимальной рабочей версии — это:

1. использование каркаса CDM `CF1..CF6`,
2. контекстно-управляемая применимость,
3. распределенное исполнение с локальным commit-point,
4. формализованная координация/эскалация,
5. обучаемая policy-система через `C_meta`.

---

## 14. References

- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-Taxonomy-Canonical](./MMCF-Conflict-Taxonomy-Canonical.md)
- [MMCF-CDM-Alignment-Matrix](./MMCF-CDM-Alignment-Matrix.md)
- [CDM PhaseTransition Overview](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition_Overview.md)
- [CDM PhaseTransition-LC](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-LC.md)
- [CDM PhaseTransition-CF](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
