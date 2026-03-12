---
title: "MMCF: Delta Registry and ChangeFlow Promotion Profile"
date: 2026-03-12
tags: [MMCF, Delta, registry, promotion, ChangeFlow, methodology]
status: profile-draft
---

# MMCF: Delta Registry and ChangeFlow Promotion Profile

## 1. Scope

Документ задает pre-flow слой MMCF над каноническим `ChangeFlow`.

Он не переопределяет `ChangeFlow-6`, `Intent`, `Context` или runtime-семантику
`Result`. Он фиксирует, как MMCF должен различать:

1. факт изменения или несоответствия;
2. запись этого факта;
3. triage и promotion decision;
4. materialized operational work unit в форме terminal `ChangeFlow`.

---

## 2. Core Route

Нормативный маршрут:

`Event -> Delta -> DeltaRecord -> DeltaRegistry -> Qualification/Triage -> PromotionDecision -> Queued ChangeFlow -> Todo -> CF1..CF6`

Где:

1. `Event` — фактическое наблюдение, событие или изменение мира;
2. `Delta` — структурное несоответствие, выделенное из события;
3. `DeltaRecord` — зафиксированная запись о конкретной `Delta`;
4. `DeltaRegistry` — множество `DeltaRecord`;
5. `Qualification/Triage` — слой классификации и решения;
6. `PromotionDecision` — решение о downstream materialization;
7. `Queued ChangeFlow` — уже созданный terminal work unit, еще не вошедший в
   активное исполнение фазы `CF1`.

---

## 3. Primary Objects

### 3.1 `Event`

`Event` — это факт наблюдения или изменения состояния.

Примеры:

1. изменился внешний номер;
2. появился новый регуляторный риск;
3. документ стал противоречить `canon`;
4. обнаружилась новая возможность, которой раньше не было.

Сам по себе `Event` еще не является `ChangeFlow` и не обязан становиться
отдельным task/work item.

### 3.2 `Delta`

`Delta` — это структурное несоответствие между актуальным и референсным
состоянием в активном контексте.

Формально:

`Delta := diff(S_actual, S_ref | C_active)`

Где `S_ref` может означать:

1. требуемое состояние;
2. допустимое состояние;
3. ожидаемое состояние;
4. предпочтительное состояние.

Следствие:

- `Delta` покрывает и деградацию, и opportunity-driven change;
- `Delta` является первичным методологическим объектом MMCF на pre-flow слое.

### 3.3 `DeltaRecord`

`DeltaRecord` — это зафиксированная запись одной конкретной `Delta`.

Минимально различимы:

1. `delta_record_id`;
2. `delta_ref` или краткое описание `Delta`;
3. `carrier_ref`, если он уже различим;
4. `source_event_ref`;
5. `context_guess` или `active_context`, если уже известен;
6. `evidence_refs`;
7. `recorded_at`;
8. текущий `triage_state`.

`DeltaRecord` не является terminal issue, coordination issue или `PTSubTask`.

### 3.4 `DeltaRegistry`

`DeltaRegistry` — это реестр зафиксированных `DeltaRecord`.

Нормативно:

1. `DeltaRegistry` не равен очереди `ChangeFlow`;
2. `DeltaRegistry` не является каноническим термином issue-tracker;
3. team-facing слово `backlog` допустимо только как domain alias для
   `DeltaRegistry`, а не как имя terminal issue status.

### 3.5 `Qualification`

`Qualification` — это управленческая классификация `Delta`, которая нужна
до materialization `ChangeFlow`.

Минимально различимы:

1. `state-conflict`;
2. `context-conflict`;
3. `constraint-conflict`;
4. `authority-conflict`;
5. `resource-conflict`;
6. `opportunity`;
7. `monitor-only`.

Класс `conflict` является частным случаем квалификации `Delta`, а не ее
универсальным синонимом.

Полная taxonomy конфликтных классов задается в:
[MMCF-Conflict-Taxonomy-Canonical](./MMCF-Conflict-Taxonomy-Canonical.md).

### 3.6 `Triage`

`Triage` — это решение о том, что делать с `DeltaRecord`.

Минимальные исходы:

1. `drop`
2. `park`
3. `merge`
4. `split`
5. `promote`

### 3.7 `PromotionDecision`

`PromotionDecision` — это решение перевести `DeltaRecord` в materialized
operational work unit.

Нормативно:

1. `promote` создает terminal `ChangeFlow`;
2. `drop`, `park`, `merge` и `split` не создают terminal `ChangeFlow`
   автоматически;
3. `Accepted` является допустимым именем verdict на triage-слое, но не
   заменяет operational state work unit.

### 3.8 `Queued ChangeFlow`

`Queued ChangeFlow` — это terminal work unit, который уже materialized после
`PromotionDecision=promote`, но еще не вошел в активное фазовое исполнение.

Нормативно:

1. `Queued` является pre-start состоянием terminal work unit;
2. `Queued` не означает, что `DeltaRegistry` уже исчерпан;
3. `Queued` не заменяет `Todo`;
4. `Queued` отличается от `Accepted`:
   - `Accepted` — verdict triage/promotion;
   - `Queued` — operational state уже созданного terminal issue.

---

## 4. Normative Rules

1. Не каждая `Delta` обязана стать active `ChangeFlow`.
2. Одна `Delta` может привести к `0..N` `ChangeFlow`.
3. Несколько `DeltaRecord` могут быть сведены в один `ChangeFlow` только если
   они действительно сходятся к одному carrier contour и одной базовой
   intent-линии.
4. Terminal issue появляется только после `PromotionDecision=promote`.
5. Raw `DeltaRecord` не должен автоматически materialize'иться в terminal
   issue только потому, что он зафиксирован.
6. `backlog` не должен использоваться как базовый методологический статус
   terminal `ChangeFlow`.
7. Для terminal `ChangeFlow` на pre-start слое используется `Queued`, а не
   `Backlog`.

---

## 5. Work Object Boundary

На уровне MMCF различайте:

1. `DeltaRecord`
   Зафиксированная `Delta`, еще не work unit.
2. coordination issue
   Tool-level контейнер для решений, чек-листов или follow-up coordination.
3. terminal issue
   Один полный `ChangeFlow`.
4. `PTSubTask`
   Один explicit non-trivial `PhaseTransition` внутри уже существующего
   terminal issue.

Следствие:

- `DeltaRecord` не должен притворяться terminal issue;
- coordination issue не должен притворяться `DeltaRegistry`;
- `PTSubTask` не должен притворяться новым `ChangeFlow`.

---

## 6. Anti-Patterns

Запрещено или нежелательно:

1. использовать `backlog` как универсальное имя и для `DeltaRegistry`, и для
   terminal issue status;
2. создавать terminal issue для каждой сырой фиксации `Delta`;
3. считать `Accepted` и `Queued` одним и тем же смысловым состоянием;
4. считать `trigger` raw-объектом мира вместо policy-signal;
5. считать `conflict` обязательным именем любой `Delta`;
6. терять связь между `DeltaRecord` и downstream `ChangeFlow`, когда promotion
   все-таки произошел.

---

## 7. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Conflict-Taxonomy-Canonical](./MMCF-Conflict-Taxonomy-Canonical.md)
- [MMCF-Term-Legacy-Mapping](./MMCF-Term-Legacy-Mapping.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Planning-Assignment-Capability-Boundary](./MMCF-Planning-Assignment-Capability-Boundary.md)
- [CDM Intent Spec](../../fcdm-core/theory/cdm/Specifications/Intent-1.1.5.md)
- [CDM ChangeFlow-6 Canonical](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Context Canonical](../../fcdm-core/theory/cdm/Specifications/Context/Context-Canonical.md)
- [CDM MetaChangeFlow Trigger Policy](../../fcdm-core/theory/cdm/Specifications/System/MetaChangeFlow-Trigger-Policy.md)
