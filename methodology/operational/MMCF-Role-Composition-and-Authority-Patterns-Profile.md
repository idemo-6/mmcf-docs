---
title: "MMCF: профиль композиции ролей и authority patterns"
date: 2026-03-12
tags: [MMCF, roles, authority, profile, level-2]
status: profile-draft
---

# MMCF: профиль композиции ролей и authority patterns

## 1. Scope

Этот документ задаёт второй уровень ролевой модели MMCF.

Он не вводит новый альтернативный role-stack, а развивает уже существующий
первый уровень, определенный в:

- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)

Если `Level 1` задает:

1. фазовые роли `CF1..CF6`;
2. ownership stack по уровням;
3. gateway-layer и фазовые переходы;

то текущий `Level 2` задает:

1. композицию фазовых ролей;
2. распределение authority;
3. паттерны делегирования;
4. требования независимости;
5. agent-neutral mapping ролей на реальных исполнителей;
6. scoped precedence и disagreement protocol для ownership/authority.

---

## 2. Нормативная граница

Этот документ является профилем, а не каноном.

Он не переопределяет:

1. канонические фазы `CF1..CF6`;
2. `ChangeFlow-6`;
3. `PhaseTransition`;
4. уже зафиксированные в `Level 1` роли:
   - `LevelOwner`
   - `LCPhaseOwner`
   - `CFPhaseOwner`
   - `TransitionOwner`
   - `TransitionExecutor`

Новый термин этого профиля:

- `CFOwner` — owner целостного `ChangeFlow` как operational work unit.

Нормативные основания:

- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Minimal-Working-Model](../core/MMCF-Minimal-Working-Model.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Intent](../../../fcdm-core/theory/cdm/Specifications/Intent-1.1.5.md)
- [CDM ChangeOperators Access & Phase Mapping](../../../fcdm-core/theory/cdm/Specifications/Operators/ChangeOperators-Access-Profile.md)

---

## 3. Базовый принцип

Роль в MMCF трактуется как функция в морфологии изменения, а не как человек,
должность или подразделение.

Следовательно:

1. одна роль может исполняться человеком, командой, сервисом или AI-агентом;
2. один агент может совмещать несколько ролей;
3. допустимость совмещения определяется не природой агента, а policy,
   risk-profile и требованиями независимости.

---

## 4. Иерархия ролей

### 4.1 Role hierarchy

| Уровень роли | Объект ответственности | Что контролирует |
|---|---|---|
| `LevelOwner(L)` | уровень `Global / Domain / Local` | общий policy/authority contour уровня |
| `LCPhaseOwner(L, F_i)` | активная фаза `LifeCycle` на уровне `L` | режим существования системы и envelope допустимых изменений |
| `CFOwner(L, flow_id)` | целостный `ChangeFlow` на уровне `L` | continuity, completion и целевую связность всего потока |
| `CFPhaseOwner(L, CF_i)` | конкретная фаза `ChangeFlow` на уровне `L` | исполнение фазовой функции внутри текущего lifecycle-envelope |
| `TransitionOwner(L, PT)` | `LC-PT` или `CF-PT` | валидность, closure и failure semantics перехода |
| `TransitionExecutor` | process-based transition | фактическое выполнение handoff/передачи |

### 4.2 Ключевая зависимость

Базовая operational-вертикаль:

`LevelOwner -> LCPhaseOwner -> CFOwner -> CFPhaseOwner`

При этом:

1. `LevelOwner` задает общий authority/policy contour уровня;
2. `LCPhaseOwner` является высшим operational звеном внутри уровня, кроме
   `LevelOwner`;
3. `CFOwner` владеет целостностью одного `ChangeFlow` как work unit и держит
   его цель, continuity и closure;
4. `CFPhaseOwner` действует внутри рамки текущей `LC`-фазы и в рамках
   конкретного потока, за который отвечает `CFOwner`;
5. `TransitionOwner` не является "ниже" фазы, а образует orthogonal line
   ответственности за переход;
6. `TransitionExecutor` не становится authority-role автоматически.

### 4.3 Практическое следствие

Нормативно:

1. `CFOwner` не заменяет `LCPhaseOwner`, а действует внутри его envelope;
2. `CFPhaseOwner` не полностью автономен ни относительно `LCPhaseOwner`, ни
   относительно `CFOwner`;
3. `CF4/Decider` не может валидно выбрать траекторию вне envelope текущей
   `LC`-фазы без escalation или policy override;
4. `CF5/Implementer` не может валидно зафиксировать изменение вне authority и
   applicability contour, заданного уровнем и active `LC phase`;
5. `CF6/Evaluator` обязан учитывать влияние не только на локальный исход, но и
   на `LC`.

### 4.4 Что такое `CFOwner`

`CFOwner` — это владелец одного целостного `ChangeFlow`, а не владелец одной
его фазы.

Минимальная функция `CFOwner`:

1. удерживать единство intent-линии потока;
2. следить, что все фазы и переходы относятся к одному и тому же flow contour;
3. обеспечивать closure потока через валидный `CF6`;
4. инициировать `repeat`, если continuation требуется немедленно;
5. держать общую ответственность за work unit, даже если отдельные фазы
   исполняются разными агентами.

`CFOwner` ближе всего к "ответственному за задачу" или "владельцу потока", но:

1. не тождественен `assignee`;
2. не обязан лично исполнять фазы;
3. не становится автоматически `DecisionAuthority`, `CommitAuthority` или
   `EvaluationAuthority`.

### 4.5 Чем `CFOwner` не должен быть

`CFOwner` не должен читаться как:

1. универсальный "начальник всех ролей" внутри потока;
2. синоним `CFPhaseOwner` любой конкретной фазы;
3. скрытый `PolicyAuthority` уровня;
4. субъект, который может единолично обходить hard-stop, `LC` envelope или
   canonical `Applicability`;
5. просто `assignee` в issue-tracker без continuity/closure обязанностей.

Следствие:

- `CFOwner` держит поток как целостную operational единицу, но не подменяет
  собой ни policy, ни lifecycle-envelope, ни phase-local валидацию.

### 4.6 Минимальный operational contract `CFOwner`

Для каждого materialized `ChangeFlow` должен быть различим один активный
`CFOwner`.

Минимальный контракт потока:

1. `flow_id`
2. `carrier_ref` или `work_unit_ref`
3. `delta_ref`
4. `intent_ref`
5. `cfowner_ref`
6. `authority_map_ref`
7. `current_lc_phase`
8. `current_cf_phase`
9. `escalation_path_ref`
10. `closure_rule`

Минимальные обязанности `CFOwner`:

1. удерживать один идентифицируемый flow contour;
2. не допускать silent-loss ownership между фазами;
3. инициировать escalation, если phase/gateway/authority disagreement ломает
   continuity потока;
4. обеспечивать валидную terminal boundary (`done/repeat/final/delayed` или
   иной разрешенный exit), а не оставлять поток без closure semantics.

Нормативно:

1. у одного materialized `ChangeFlow` в момент времени должен быть не более
   одного активного `CFOwner`;
2. смена `CFOwner` требует явного handoff с trace;
3. поток без различимого `CFOwner` считается operationally degraded;
4. governance-heavy, multi-agent или high-risk flow не должен входить в
   `CF5` или final `CF6` без различимого `CFOwner`.

### 4.7 Scoped precedence between roles

Вертикаль

`LevelOwner -> LCPhaseOwner -> CFOwner -> CFPhaseOwner`

не должна читаться как "абсолютная командная лестница" для любого вопроса.

Это scoped precedence:

| Вопрос | Primary scope owner | Что не может быть overridden снизу |
|---|---|---|
| допустим ли класс изменения по policy | `LevelOwner / PolicyAuthority` | policy envelope уровня |
| допустим ли ход внутри текущей `LC`-фазы | `LCPhaseOwner` | lifecycle envelope и phase-gates |
| относится ли действие к данному flow, нужен ли repeat/new flow | `CFOwner` | continuity, identity и closure потока |
| валиден ли phase output и phase completion | `CFPhaseOwner` | локальная phase-логика внутри разрешенного envelope |
| может ли закрыться переход | `TransitionOwner / GatewayApprovalAuthority` | closure semantics конкретного `PT` |
| каков итоговый `Result` и terminal evaluation verdict | `Evaluator / EvaluationAuthority` | final result boundary `CF6` |

Следствия:

1. `CFPhaseOwner` не может валидно объявить phase-complete ход, который
   выводит поток за `LC`-envelope;
2. `CFOwner` не может переопределить phase-local validity только потому, что
   "поток надо двигать";
3. `LCPhaseOwner` не решает вместо `Evaluator` сам итоговый `Result`, если
   поток уже дошел до корректного `CF6`;
4. `LevelOwner/PolicyAuthority` не должен подменять собой обычную phase
   execution, кроме explicit override.

### 4.8 Minimal handover rule for `CFOwner`

При смене `CFOwner` должны быть зафиксированы минимум:

1. `from_cfowner`
2. `to_cfowner`
3. `reason`
4. `effective_at`
5. `current_phase_snapshot`
6. `open_conflicts_or_blockers`

Без этого continuity потока считается неаудируемой.

---

## 5. Фазовые роли как минимальное ролевое ядро

### 5.1 Canonical role kernel

| Canonical phase | Role function | Минимальная функция |
|---|---|---|
| `CF1` | `Collector` | фиксация входного контура и исходных ограничений |
| `CF2` | `Analyzer` | интерпретация и построение модели текущего состояния |
| `CF3` | `Forecaster` | генерация допустимых траекторий и альтернатив |
| `CF4` | `Decider` | выбор траектории и фиксация rationale |
| `CF5` | `Implementer` | локальный commit изменения |
| `CF6` | `Evaluator` | фиксация `Result`, обновление `Experience` и влияние на `LC` |

Это ядро не заменяется никакими domain aliases и не переопределяется текущим
профилем.

### 5.2 Distributed CF principle

Распределенный `ChangeFlow` допустим и ожидаем:

1. `CF1`, `CF2`, `CF3`, `CF4`, `CF6` могут исполняться различными агентами или
   контурами;
2. `CF5` остаётся локальным commit-point carrier system;
3. `CF6` может быть локальным, доменным, родительским или hybrid, если это не
   ломает единую final result boundary.

---

## 6. Композиция фазовых ролей

### 6.1 Общий принцип

Композиция ролей допустима по умолчанию, если одновременно:

1. сохраняется аудитируемость решений;
2. не теряется различимость phase outputs;
3. совмещение не ломает policy или требования независимости.

### 6.2 Типовые допустимые композиции

Обычно допустимы:

1. `Collector + Analyzer`
2. `Analyzer + Forecaster`
3. `Collector + Analyzer + Forecaster`
4. `TransitionOwner + TransitionExecutor`, если риск низкий и handoff прозрачен
5. `CF1..CF3` в одном аналитическом контуре

### 6.3 Рискованные композиции

Требуют явной policy или override:

1. `Forecaster + Decider`
2. `Decider + Implementer`
3. `Implementer + Evaluator`
4. `DecisionAuthority + CommitAuthority`
5. `ClaimAuthority + Evaluator` в claim-bearing high-risk flow

### 6.4 Предпочтительные разведения

Предпочтительно разводить:

1. `Decider` и `Evaluator` в governance-heavy или high-risk flow;
2. `Implementer` и `Evaluator` там, где важна независимая оценка;
3. `TransitionOwner` и `TransitionExecutor` в process-based critical gateway;
4. `CF4` и `CF5`, если цена ошибки коммита высока.

---

## 7. Authority allocation

### 7.1 Принцип

Исполнение роли и authority по этой роли не обязаны совпадать.

Различайте:

1. `role executor`
2. `role owner`
3. `role authority`

### 7.2 Минимальные authority-aspects

Поверх `Level 1` вводятся следующие authority-aspects:

1. `DecisionAuthority`
2. `CommitAuthority`
3. `EvaluationAuthority`
4. `PolicyAuthority`
5. `GatewayApprovalAuthority`
6. `ClaimAuthority`, когда flow несет claims

Это не новые канонические фазы и не новый role-stack.  
Это authority-aspects поверх уже существующих phase и transition roles.

### 7.3 Базовое распределение

| Authority-aspect | Обычно связан с | Ограничение |
|---|---|---|
| `DecisionAuthority` | `CF4 / Decider` | действует внутри envelope текущей `LC`-фазы |
| `CommitAuthority` | `CF5 / Implementer` | commit допустим только локально у carrier system |
| `EvaluationAuthority` | `CF6 / Evaluator` | обязан фиксировать итоговый `Result` и влияние на `LC` |
| `PolicyAuthority` | `LevelOwner` или делегированный policy owner | не подменяет собой phase execution по умолчанию |
| `GatewayApprovalAuthority` | `TransitionOwner` или внешний approving contour | нужен только для approve-bearing transitions |
| `ClaimAuthority` | governance/evidence contour | включается только для claim-bearing flows |

### 7.4 Связь `CFOwner` и authority

По умолчанию `CFOwner`:

1. отвечает за целостность потока;
2. не обязан обладать всеми authority-aspects потока;
3. может совпадать с `DecisionAuthority` или `EvaluationAuthority`, но это не
   является обязательным правилом.

Если `DecisionAuthority`, `CommitAuthority`, `EvaluationAuthority`,
`GatewayApprovalAuthority` или `ClaimAuthority` выдают несовместимые verdicts,
это должно классифицироваться как `authority-conflict`, а не как "общая
организационная путаница".

Практически:

1. low-risk flow часто допускает `CFOwner = Decider`;
2. normal flow часто допускает `CFOwner = Decider`, но предпочитает
   различимость `Evaluator`;
3. governance-heavy flow часто требует, чтобы `CFOwner` координировал поток,
   но authority была распределена между несколькими ролями.

### 7.5 Authority disagreement and override protocol

Если authority-aspects дают несовместимые verdicts, должен включаться
explicit disagreement protocol.

Минимальная последовательность:

1. классифицировать disagreement как `authority-conflict`;
2. зафиксировать `conflict_parties[]` и затронутый scope:
   - `decision`
   - `commit`
   - `evaluation`
   - `gateway-approval`
   - `claim`
   - `policy`
3. остановить affected action, если конфликт затрагивает:
   - `CF4 -> CF5`
   - `CF5`
   - `CF6`
   - closure критического `PT`
4. применить scoped precedence из раздела 4.7;
5. если scoped precedence не дает однозначного inline resolution, эскалировать
   в `C_coord / C_meta` по policy-пути;
6. если требуется override, зафиксировать его как отдельное audited событие;
7. после resolution выполнить revalidation affected phase/transition.

Допустимые исходы:

1. `resolved-inline`
2. `resolved-by-override`
3. `escalated`
4. `blocked`

Нормативно:

1. `authority-conflict` не должен silently collapse'иться в решение
   `assignee` или самого громкого стейкхолдера;
2. unresolved `authority-conflict`, затрагивающий commit или final evaluation,
   блокирует движение вперед;
3. `CFOwner` обязан инициировать protocol, но не обязан быть финальным
   resolving authority.

---

## 8. Паттерны делегирования

### 8.1 Фазовые паттерны

Типовые валидные паттерны:

1. `Centralized Decision`
   `CF1..CF3` распределены, `CF4` централизован.
2. `Local Commit`
   `CF5` всегда остается у локального carrier contour.
3. `Delegated Evaluate`
   финальный `CF6` делает дочерний контур.
4. `Centralized Evaluate`
   финальный `CF6` делает доменный или родительский контур.
5. `Hybrid Evaluate`
   локальный pre-eval + внешний final eval.
6. `Process-Based Gateway`
   переход исполняет отдельный `TransitionExecutor`.

Во всех этих паттернах `CFOwner` может оставаться единым owner всего потока,
даже если phase executors и authorities распределены.

### 8.2 Ограничение локальности `CF5`

Даже при сильной распределенности предшествующих фаз:

1. `CF5` остается локальной точкой причинности;
2. локальный authority-agent carrier system выполняет commit;
3. внешний контур может готовить, разрешать или ограничивать commit, но не
   делает сам carrier-local `CF5` за систему.

### 8.3 `CF5-only` как делегированная исполнительная проекция

Допускается:

1. upstream-контур берет `CF1..CF4` и `CF6`;
2. локальный контур выполняет только `CF5`;
3. перед этим требуется валидный attest и policy-consistent commit authority.

---

## 9. Паттерны независимости

### 9.1 Общий принцип

Чем выше risk, governance load, claim load или цена ошибки, тем выше
предпочтительная независимость между:

1. выбором;
2. коммитом;
3. оценкой;
4. approval/gateway closure.

### 9.2 Минимальная шкала

| Flow class | Обычно допустимо совмещать | Предпочтительно различать | Должно быть различимо | Минимум operational trace |
|---|---|---|---|---|
| `low-risk` | `Collector + Analyzer + Forecaster`; `Forecaster + Decider`; `Decider + Implementer`; `TransitionOwner + TransitionExecutor` | `Evaluator` хотя бы в комментарии closure, если он не совпадает с исполнителем | `CFOwner` как различимая функция потока, даже если совпадает с одним агентом | `cfowner_ref`, текущий `assignee/executor`, active phase |
| `normal` | `Collector + Analyzer`; `Analyzer + Forecaster`; иногда `CFOwner = Decider` | `Decider != Evaluator`; `TransitionOwner` различим от phase executors хотя бы в trace; `CommitAuthority` различим, если commit materially significant | `CFOwner` не должен silently collapse'иться в простой `assignee`; authority map должен быть читаем | `cfowner_ref`, `assignee_ref`, `decision_authority`, `evaluation_authority`, active `TransitionOwner` при нетривиальном `PT` |
| `high-risk / claim-bearing / governance-heavy` | только ранние аналитические композиции (`CF1..CF3`) при сохранении auditability | `Decider != Implementer`; `Implementer != Evaluator`; `GatewayApprovalAuthority != TransitionExecutor`; `ClaimAuthority != обычный phase executor` | `CFOwner`, `CommitAuthority`, `EvaluationAuthority`, `GatewayApprovalAuthority`, `ClaimAuthority` должны быть явно различимы там, где они релевантны; unresolved `authority-conflict` блокирует ход | полный role trace, authority map, handoff refs и conflict trace |

### 9.3 Operational rules for risk-based independence

Нормативно:

1. risk class может повышаться по мере хода потока, но не должен silently
   понижаться без явного rationale;
2. если один агент несет несколько ролей, trace должен различать сами роли, а
   не только имя агента;
3. если required distinction для текущего risk class не может быть обеспечено,
   поток должен считаться `governance-heavy` или `operationally degraded` до
   явного override;
4. для normal и high-risk flow оценка допустимых совмещений должна делаться не
   позже intake/promote момента materialized work unit;
5. `assignee` в tool-layer никогда не заменяет сам по себе `CFOwner`,
   authority map или проверку независимости.

---

## 10. Agent-neutral mapping

### 10.1 Допустимые носители ролей

Роль может исполняться:

1. человеком;
2. командой или отделом;
3. программным сервисом;
4. AI-агентом;
5. hybrid chain.

### 10.2 Ограничение профиля

Текущий профиль остаётся agent-neutral.

Он не утверждает, что:

1. authority по умолчанию принадлежит человеку;
2. authority по умолчанию принадлежит AI;
3. организационная должность тождественна роли.

Он утверждает только:

1. роль — это функция изменения;
2. authority может быть динамически распределен;
3. конкретное распределение определяется policy и capability contour.

IDEMO-specific или post-anthropocentric интерпретации могут строиться
поверх этого профиля отдельными mapping-документами.

---

## 11. Authority modes by phase and transition

### 11.1 Базовые режимы

Используются режимы:

1. `auto`
2. `hybrid`
3. `manual`

### 11.2 Типовые соответствия

Типовые, но не абсолютные, соответствия:

1. `CF1/CF2` чаще допускают `auto` или `hybrid`
2. `CF3` часто допускает `auto`, но при high-stakes требует review
3. `CF4` чаще является `hybrid` или `manual`
4. `CF5` зависит от commit-policy и локального authority contour
5. `CF6` в claim-bearing или governance-heavy flow предпочитает `hybrid` или
   `manual`
6. `PT(CF5->CF6)` может быть `approve`-bearing и разрешать outcome по policy
   или authority verdict

### 11.3 Rule

Authority mode не меняет каноническую фазу.

Он определяет только:

1. кто может завершить фазу;
2. какой вид подтверждения нужен;
3. возможен ли auto-close без внешнего approve.

---

## 12. Minimal acceptance rules for Level 2

Профиль считается применённым корректно, если:

1. `Level 1` не переопределён и не продублирован;
2. явно проведена вертикаль `LevelOwner -> LCPhaseOwner -> CFOwner -> CFPhaseOwner`;
3. `LCPhaseOwner` зафиксирован как высшее operational звено внутри уровня,
   кроме `LevelOwner`;
4. `CFOwner` различён с `CFPhaseOwner` и не редуцирован до простого assignee;
5. `TransitionOwner` и `TransitionExecutor` не смешаны с phase ownership;
6. role composition не путается с authority allocation;
7. agent-neutral mapping сохранен;
8. локальность `CF5` сохранена;
9. `CF6` сохраняется как final result boundary даже при delegated или hybrid
   evaluation;
10. у materialized flow различим один активный `CFOwner`;
11. scoped precedence не подменена неявной орг-иерархией;
12. для `authority-conflict` определен explicit disagreement protocol.

---

## 13. References

- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Minimal-Working-Model](../core/MMCF-Minimal-Working-Model.md)
- [MMCF-Conflict-and-Applicability-Profile](../context/MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Conflict-Taxonomy-Canonical](../context/MMCF-Conflict-Taxonomy-Canonical.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Context-Coordination-and-Meta-Policy](../context/MMCF-Context-Coordination-and-Meta-Policy.md)
- [POSITIONING_AND_ADOPTION_ROADMAP](../../positioning/POSITIONING_AND_ADOPTION_ROADMAP.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Intent](../../../fcdm-core/theory/cdm/Specifications/Intent-1.1.5.md)
- [CDM ChangeOperators Access & Phase Mapping](../../../fcdm-core/theory/cdm/Specifications/Operators/ChangeOperators-Access-Profile.md)
