---
title: "MMCF Delivery: профиль планирования для Linear"
date: 2026-03-12
tags: [MMCF, delivery, Linear, planning, profile, ChangeFlow]
status: profile-draft
---

# MMCF Delivery: профиль планирования для Linear

## 1. Область

Этот документ определяет аддитивное расширение планирования `v1.1` для
терминальных задач в Linear, используемых в MMCF/CDM-совместимых системах
delivery.

Он дополняет, а не заменяет:

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)

Общая MMCF-граница между task planning, executor matching и measured capability
определена отдельно в:

- [MMCF-Planning-Assignment-Capability-Boundary](../operational/MMCF-Planning-Assignment-Capability-Boundary.md)
- [MMCF-Executor-Matching-and-Capability-Registry-Profile](../operational/MMCF-Executor-Matching-and-Capability-Registry-Profile.md)

Этот профиль покрывает только task-side planning signals, которые действительно
имеет смысл хранить на issue.

Bridge для legacy `CI/V` материалов и более общего creativity/variability
профиля определен отдельно в:

- [MMCF-Delivery-Planning-CI-V-Bridge](./MMCF-Delivery-Planning-CI-V-Bridge.md)

---

## 2. Граница совместимости

1. Одна терминальная задача по-прежнему равна ровно одному `ChangeFlow`.
2. Базовая схема Linear `v1` остается минимально обязательной.
3. Этот профиль добавляет сигналы планирования и не переопределяет семантику
   `Result`, `Applicable`, `exit_decision` или phase status.
4. `Doubt`, `Subjectivity` и capability matching здесь не трактуются как issue fields.
5. Реальная креативность полученного решения остается наблюдением стороны
   `evaluate`, а не обязательным intake-scalar.
6. Пакет навыков Codex для Linear трактует этот профиль как аддитивный:
   intake заполняет planning-данные, repeat сохраняет их по умолчанию, а
   advance/close не переписывают planning-assumptions без явного запроса.
7. Подбор исполнителя и capability registry остаются вне issue-state и не
   относятся к этому tool-profile.

---

## 3. Выравнивание с canon

Этот профиль планирования опирается на следующие канонические ограничения:

1. `Creative ChangeFlow` является режимом исполнения, а не общей меткой
   сложности.
2. `Doubt` может объяснять выбор planning-policy или matching-policy, но не
   становится обязательным issue-field в этом профиле.
3. `Subjectivity` осмысленна только относительно другой системы или `Ref` в
   фиксированном контексте и потому не хранится здесь как task parameter.
4. Оценку creativity в software нельзя редуцировать к одному `S`; содержательный
   паттерн оценки таков: `R + S(A,Ref,C)`.
5. `Variativity Target` считает положительные, реализуемые варианты,
   которые нужно сравнить до `CF4`; каноническая ветка `inapplicable/stop` не
   считается одной из этих альтернатив.

---

## 4. Task-side planning на issue

Рекомендуемые поля планирования задачи:

| Поле | Хранение в `v1.1` | Тип | Допустимые значения | Правило |
|---|---|---|---|---|
| `Flow Mode` | custom field | enum | `standard / creative` | Обязательно на intake. |
| `Variativity Target` | custom field | enum | `1 / 2 / 3 / 4+` | Обязательно на intake; может быть пересмотрено до закрытия `CF3`. |
| `Decide Policy` | custom field | enum | `normal / delayed` | Обязательно на intake. |
| `max_decide_delay` | planning block | duration/text | policy-defined | Обязательно при `Flow Mode=creative` или `Decide Policy=delayed`. |
| `forced_collapse_trigger` | planning block | text/ref | workspace-defined | Обязательно при `Flow Mode=creative`. |
| `resource_cap` | planning block | text/ref | workspace-defined | Рекомендуется для creative или `Variativity Target > 1`. |

Нормативное правило:

- `Flow Mode`, `Variativity Target` и `Decide Policy` описывают планируемый
  контур исполнения задачи.
- Для `Linear UI` `Variativity Target` фиксируется как enum `1 / 2 / 3 / 4+`.
- Если фактическая рабочая потребность позже потребует более точного
  численного `Variativity Target`, это должно быть оформлено как отдельный
  future upgrade, а не как скрытое нарушение текущей UI-схемы.
- `CI` из более общего creativity/variability profile по-прежнему может
  использоваться как аналитический скаляр, но в схему issue `v1.1` он не
  продвигается; для перевода legacy `CI/V` используйте отдельный bridge.

### 4.1 Что не хранится на issue

Следующие элементы не являются task-side parameters и не должны храниться на
issue как часть planning `v1.1`:

1. `Doubt`
2. `Subjectivity`
3. assignment bands по фазам
4. measured executor capability
5. candidate ranking или suitability scores
6. `Creativity_actual`

Если workspace использует механизмы подбора исполнителя, они должны читать
task-side planning signals задачи и работать во внешнем matching/capability
слое, а не превращать внутренние расчеты подбора в поля issue.

---

## 5. Политика custom fields для `v1.1`

Рекомендуемое расширение custom fields для первого профиля планирования:

1. `Flow Mode`
2. `Variativity Target`
3. `Decide Policy`

Эти три пункта выносятся наверх, потому что они:

1. относятся к стороне задачи, а не к стороне исполнителя;
2. компактны и пригодны для отчетности;
3. достаточно стабильны для фильтрации и дашбордов.

Следующие элементы остаются вне custom fields в `v1.1`:

1. `max_decide_delay`
2. `forced_collapse_trigger`
3. `resource_cap`
4. все measured executor values и matching outputs

Причина:

- эти значения либо высокоэнтропийны, либо текстуально зависят от правил, либо относятся к
  внешнему реестру возможностей, а не к самой ontology issue.

---

## 6. Опциональная note о `MetaCF Risk`

`MetaCF Risk` сохраняется как допустимый концепт методологического уровня, но
в `delivery/linear` больше не является обязательным или рекомендуемым issue
field.

Если команде нужно явно отметить риск reframing/escalation, это можно делать
только как краткую note в `Planning rationale`, а не как отдельный field.

Рекомендуемые формулировки note:

1. `Low meta-level reframing risk`
2. `Moderate risk of reframing during forecast/decide`
3. `High risk of escalation into MetaChangeFlow`

---

## 7. Влияние на оценку и бюджет времени

Этот профиль рекомендует бюджетировать в planning units, а затем отображать
их в часы или story points через калибровку команды.

### 7.1 Рекомендуемая формула по умолчанию

Пусть:

- `B` = базовая оценка терминальной задачи без planning-extension;
- `V_map` = числовое отображение `Variativity Target`:
  - `1 -> 1`
  - `2 -> 2`
  - `3 -> 3`
  - `4+ -> 4` по умолчанию.

Рекомендуемая аддитивная модель:

`PlanningUnits = B + (V_map - 1) + ceil((V_map - 1) / 2) + C + D`

Где:

1. `(V_map - 1)` дает дополнительную нагрузку `forecast` для дополнительных
   вариантов;
2. `ceil((V_map - 1) / 2)` дает дополнительную нагрузку `decide` на сравнение
   альтернатив;
3. `C = 1`, если `Flow Mode=creative`, иначе `0`;
4. `D = 1`, если `Decide Policy=delayed`, иначе `0`.

Интерпретация:

1. `Variativity Target` прежде всего растягивает `CF3` и `CF4`.
2. `Flow Mode=creative` добавляет обязательный дополнительный объем исследования еще до
   учета любых дополнительных альтернатив.
3. `Decide Policy=delayed` добавляет decision overhead и требует явного
   контроля через `max_decide_delay`.

Затраты на подбор исполнителя, alignment или review можно оценивать отдельно,
но они не являются частью issue-side planning schema `v1.1`.

Если `Planning rationale` явно указывает на высокий риск reframing или
эскалации, команда может добавить contingency вручную, но это не является
частью минимальной формулы.

### 7.2 Сводка влияния по фазам

Рекомендуемые основные эффекты по сигналам:

| Сигнал | Основное влияние на фазу | Смысл для планирования |
|---|---|---|
| `Flow Mode=creative` | `CF2..CF4` | более широкий поиск и контроль отложенного collapse |
| более высокий `Variativity Target` | `CF3`, затем `CF4` | больше вариантов для генерации и сравнения |
| `Decide Policy=delayed` | `CF4` | явное удержание альтернатив открытыми по правилам |

---

## 8. Минимальный шаблон planning-block

Рекомендуемый фрагмент issue-body:

```md
## Planning
- Flow mode: creative
- Variativity target: 3
- Decide policy: delayed
- max_decide_delay: 2 working days
- forced_collapse_trigger: No dominant path after review of 3 candidates
- resource_cap: CF3 exploration <= 5 person-days
- Planning rationale: Moderate risk of reframing during forecast/decide
```

---

## 9. Антипаттерны

Следующее недопустимо в этом профиле:

1. Использовать `creativity` как неструктурированное intake-field без
   `Flow Mode`, `Variativity Target` и предохранительных ограничителей.
2. Считать каноническую ветку `do-not-proceed/inapplicable` внутри
   `Variativity Target`.
3. Хранить measured capability, candidate scores или assignment bands прямо на
   issue, как будто это свойства задачи.
4. Выставлять `Flow Mode=creative` без обоих полей `max_decide_delay` и
   `forced_collapse_trigger`.

---

## 10. Порядок внедрения

Рекомендуемый порядок внедрения:

1. внедрить три planning custom fields;
2. внедрить структурированный planning-block в issue bodies;
3. отдельно от delivery ввести внешний matching/capability layer;
4. автоматизировать подбор assignee только после того, как matching/profile
   слой получит данные, подтвержденные evidence.

Этот порядок сохраняет чистую границу между планированием delivery и измерением
исполнителя.

TODO:

- если практика покажет устойчивую пользу от точного числового `Variativity Target`
  сверх `4+`, вернуться к вопросу о future integer-версии поля вне текущего
  `Linear UI v1.1`.

---

## 11. Нормативные ссылки

- [MMCF-Planning-Assignment-Capability-Boundary](../operational/MMCF-Planning-Assignment-Capability-Boundary.md)
- [MMCF-Executor-Matching-and-Capability-Registry-Profile](../operational/MMCF-Executor-Matching-and-Capability-Registry-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Planning-CI-V-Bridge](./MMCF-Delivery-Planning-CI-V-Bridge.md)
- [MMCF-Creativity-and-Variability-Profile](../domain/MMCF-Creativity-and-Variability-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](../operational/MMCF-Operational-Roles-and-Gateways.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Creative ChangeFlow Canonical](../../../fcdm-core/theory/cdm/Specifications/System/CreativeChangeFlow-Canonical.md)
- [CDM Doubt Canonical](../../../fcdm-core/theory/cdm/Specifications/System/Doubt-Canonical.md)
- [CDM Subjectivity Canonical](../../../fcdm-core/theory/cdm/Specifications/System/Subjectivity-Canonical.md)
- [CDM Subjectivity Reference Agent Profile](../../../fcdm-core/theory/cdm/Specifications/System/Subjectivity-Reference-Agent-Profile.md)
- [CDM Subjectivity and Creativity in Software](../../../fcdm-core/theory/cdm/Specifications/System/Subjectivity-Domain-Profiles/Subjectivity-Domain-Software-Creativity-Profile.md)
- [CDM MetaChangeFlow Model Core](../../../fcdm-core/theory/cdm/Specifications/System/MetaChangeFlow-Model-Core.md)
