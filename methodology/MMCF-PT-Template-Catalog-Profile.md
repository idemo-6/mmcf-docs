---
title: "MMCF: профиль каталога PT-шаблонов"
date: 2026-03-12
tags: [MMCF, PhaseTransition, template, catalog, profile]
status: profile-draft
---

# MMCF: профиль каталога PT-шаблонов

## 1. Назначение

Этот профиль вводит reusable слой `PT template catalog` поверх уже принятой
модели `PhaseTransition`.

Профиль нужен, чтобы:

1. не изобретать каждую explicit `PT`-конструкцию с нуля;
2. не плодить произвольные PT-типы в каноне;
3. получить мост между каноном `PT`, domain/workflow overlays и runtime
   materialization в existing tools и native systems.

Профиль не меняет канон `PT`. Он описывает только reusable operational layer.

---

## 2. Layered model

Каталог строится по такой вертикали:

1. `canonical PT class`
2. `template family`
3. `domain/workflow template`
4. `PT runtime instance`

### 2.1 `canonical PT class`

Канонический слой хранит только инвариантную семантику:

1. `pt_class = unconditional | conditional`
2. допустимые failure/retry оси

`pt_form` и `pt_scenario` не относятся к самой канонической class-оси.
Они принадлежат operational layer шаблона и runtime instance.

### 2.2 `template family`

Reusable operational family поверх канона.

Примеры:

1. `approval`
2. `claim-review`
3. `process-handoff`
4. `external-response`
5. `retry-recovery-gate`
6. `release-quality-gate`

### 2.3 `domain/workflow template`

Конкретный прикладной шаблон для domain pack или workflow profile.

Примеры:

1. `manager-approval-via-email`
2. `committee-approval`
3. `ai-authority-approval`
4. `security-review-gate`
5. `docs-canon-approval`

### 2.4 `PT runtime instance`

Фактическое исполнение перехода в конкретном потоке.

В existing tools это может быть `PTSubTask`.
В native system это должно быть first-class `PhaseTransitionInstance`.

---

## 3. Правило привязки

Шаблон привязывается не к issue вообще, а к конкретному переходу:

- `from_phase -> to_phase`

Правильно:

1. `forecast -> decide = committee-approval`
2. `implement -> evaluate = ai-authority-approval`

Неправильно:

1. `у этой задачи шаблон approve`

---

## 4. Минимальная схема PT template

### 4.1 Identity

1. `template_id`
2. `template_name`
3. `template_family`
4. `domain_or_workflow_scope`

### 4.2 Canonical binding

1. `pt_class`
2. `pt_form`
3. `pt_scenario`
4. `from_phase`
5. `to_phase`

### 4.3 Operational defaults

1. `default_runtime_state`
2. `default_failure_policy`
3. `default_materialization = implicit | explicit`
4. `default_due_rule`

### 4.4 Authority / ownership

1. `default_transition_owner_role`
2. `default_transition_executor_role`
3. `default_approval_authority_role`
4. `default_claim_authority_role`

### 4.5 Required refs

1. `required_evidence_refs`
2. `required_claim_refs`
3. `required_approval_refs`

### 4.6 Tool/runtime shaping

1. `title_template`
2. `body_template`
3. `suggested_status_model`
4. `visibility_labels`

### 4.7 Notes

1. `usage_notes`
2. `anti_patterns`

### 4.8 Что не должно жить в шаблоне

1. concrete assignee
2. concrete approver identity
3. runtime attempt history
4. actual due date
5. actual refs collected during execution

Это уже данные runtime instance.

---

## 5. Источник defaults и issue-level override

Иерархия источников defaults:

1. `canonical PT class`
2. `template family`
3. `domain/workflow template`
4. `issue-level PT plan`
5. `PT runtime instance`

Правило:

1. defaults по умолчанию приходят из policy/profile, а не от автора задачи;
2. issue может сделать override только внутри допустимого envelope;
3. runtime может показать более сильную materialization, чем было в плане.

### 5.1 Что можно override на уровне задачи

1. `default_materialization`
2. `default_failure_policy`
3. `default_due_rule`
4. `default_transition_owner_role`
5. `default_transition_executor_role`
6. `default_approval_authority_role`
7. `default_claim_authority_role`
8. `required_*_refs`
9. `title_template` / `body_template`, если tool wording реально требует адаптации

### 5.2 Что не должно override'иться на уровне задачи

1. `pt_class`
2. `pt_form`, если он уже задан шаблоном
3. `pt_scenario`, если он уже задан шаблоном
4. `template_family`
5. canonical binding `from_phase -> to_phase`

---

## 6. Materialization threshold

`PT` существует всегда по канону.
Явный runtime-object нужен не всегда.

### 6.1 Оставлять implicit

Переход можно оставлять implicit, если одновременно:

1. он короткий и не образует отдельный bottleneck;
2. нет отдельного owner/executor как значимого operational object;
3. нет отдельного approval trail;
4. нет claim-specific contour;
5. нет retry/failure lifecycle, который нужно отслеживать отдельно;
6. нет отдельного SLA / deadline / queue;
7. переход не требует самостоятельной управленческой видимости.

### 6.2 Materialize явно

Явный runtime-object нужен, если выполняется хотя бы одно:

1. transition is `approve`
2. transition is `claim`
3. transition is `approve+claim`
4. transition has separate `TransitionOwner` or `TransitionExecutor`
5. transition has its own queue / SLA / timeout / deadline
6. transition has retry/failure lifecycle worth tracking separately
7. transition became a long-lived bottleneck
8. transition crosses a distributed contour and handoff is materially significant
9. governance requires explicit visibility / auditability
10. selected template already sets `default_materialization=explicit`

### 6.3 Distributed CF note

В distributed `CF` переход operationally по умолчанию следует считать
`process-based` по оси `pt_form`, если соседние фазы принадлежат разным
агентам или контурам.

Но explicit materialization все равно требует operational significance.

---

## 7. Стартовый каталог families v1

### 7.1 Mandatory families

1. `approval`
2. `claim-review`
3. `process-handoff`
4. `external-response`

### 7.2 Strongly recommended families

1. `retry-recovery-gate`
2. `release-quality-gate`

### 7.3 Что не считать family layer

1. `email`
2. `meeting`
3. `call`
4. `chat`

Это media/channels и должно появляться только на уровне конкретного
domain/workflow template.

---

## 8. Existing tools и native systems

### 8.1 Existing tools

В existing tools:

1. selected template binding хранится рядом с parent flow;
2. explicit runtime instance materialize'ится как `PTSubTask` или иной child object;
3. labels остаются только visibility hints.

### 8.2 Native systems

В native system ожидаются first-class сущности:

1. `Flow`
2. `PhaseInstance`
3. `PhaseTransitionInstance`
4. `PTTemplateCatalog`
5. `PTTemplateBinding`

Current Linear model следует понимать как bridge к этой архитектуре.

---

## 9. References

- [MMCF-Phase-and-Transition-State-Separation-Profile](./MMCF-Phase-and-Transition-State-Separation-Profile.md)
- [MMCF-Operational-Gateway-Scenario-Profile](./MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Domain-Pack-Profile](./MMCF-Domain-Pack-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Delivery-PT-Template-Catalog-Profile](./delivery/MMCF-Delivery-PT-Template-Catalog-Profile.md)
