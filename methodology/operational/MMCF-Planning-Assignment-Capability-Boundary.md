---
title: "MMCF: Planning, Assignment, and Capability Boundary"
date: 2026-03-12
tags: [MMCF, planning, assignment, capability, profile]
status: profile-draft
---

# MMCF: Planning, Assignment, and Capability Boundary

## 1. Scope

Этот документ задает общую границу между:

1. planning-сигналами самой задачи;
2. требованиями к назначению исполнителя;
3. измеренными capability-профилями исполнителей.

Профиль не переопределяет `ChangeFlow-6`, `CreativeChangeFlow`,
`Subjectivity`, `Doubt` или `MetaChangeFlow`. Он задает только operational
boundary для их совместного применения в MMCF.

Этот документ намеренно расположен в корневом слое `methodology/`, а не в
`delivery`, потому что его границы применимы к любым tool-profiles и не
сводятся к одному issue-tracker.

---

## 2. CDM foundation

Профиль опирается на следующие положения CDM:

1. Creative mode не отменяет канонические фазы и phase-gates.
2. Для creative mode обязательны safety-hooks:
   - `max_decide_delay`
   - `forced_collapse_trigger`
3. `Subjectivity` осмысленна только относительно контекста и `Ref`.
4. `Doubt` может оправдывать расширенный `forecast/decide` или эскалацию в
   `MetaChangeFlow`, но не заменяет phase-gates.
5. Фактическая creativity оценивается по связке результата и различия
   траекторий, а не по одному intake-signal.

---

## 3. Three Planning Planes

### 3.1 Task-side planning signals

Это сигналы, описывающие ожидаемый contour исполнения самой задачи.

К ним относятся:

1. execution mode (`standard` / `creative` или эквивалентный доменный режим);
2. planned alternative width (`variativity target`, `V_effective` или доменный
   аналог);
3. decision-collapse policy (`normal`, `delayed` или профильный аналог);
4. expected meta-level risk;
5. safety-hooks режима:
   - `max_decide_delay`
   - `forced_collapse_trigger`
   - `resource_cap`, когда профиль требует ресурсный ограничитель exploration.

Нормативно:

1. эти сигналы описывают поток, а не исполнителя;
2. ветка `inapplicable` не считается положительной альтернативой внутри
   variativity target;
3. actual creativity остается результатом evaluate, а не обязательным
   intake-scalar.

### 3.2 Assignment-side requirements

Это требования к тому, какой исполнитель или контур исполнения нужен задаче.

К ним относятся:

1. требуемый уровень устойчивости к `Doubt`;
2. требуемый уровень `Subjectivity` относительно релевантного `Ref`;
3. фазово-специфичные различия между explore/decide/execute segments;
4. требования к review/alignment нагрузке, если они materially influence
   execution path.

Нормативно:

1. эти требования описывают требуемое соответствие, а не фактические measured
   values исполнителя;
2. они могут жить в отдельном assignment/matching слое, а не в runtime-state задачи;
3. сама задача может хранить только `assignee` и, при необходимости, краткую
   assignment note;
4. они не должны смешиваться с measured executor profile.

### 3.3 Measured executor capability

Это внешний, evidence-backed профиль исполнителя или исполнительного контура.

Минимально различимы:

1. `executor_id`
2. `context_scope`
3. `reference_profile_id`
4. measured bands or scores relevant to `Doubt` / `Subjectivity`
5. `tested_at`
6. `profile_version`
7. `evidence_ref`

Нормативно:

1. measured capability остается вне runtime-state самой задачи;
2. задача может хранить только `assignee` и, при необходимости, краткую
   assignment note, но не должна содержать measured registry entry;
3. stale capability profile должен помечаться как исторический или требующий
   пересмотра.

---

## 4. Separation Rules

Обязательные правила разделения:

1. task planning signals не являются measured properties исполнителя;
2. assignment requirements не являются доказанными capability-values;
3. measured executor capability не должна вталкиваться в task-state как будто
   это свойство carrier task;
4. safety-hooks режима относятся к потоку, а не к личности исполнителя;
5. `Subjectivity` без фиксированного `Ref` и контекста не используется как
   assignment signal.

---

## 5. What Belongs To Applied Profiles

В applied profiles допускается конкретная проекция этих смыслов в:

1. tool fields;
2. issue-body blocks;
3. dashboard enums;
4. automation contracts;
5. registry schemas.

Но сами названия полей, enum-values и storage choices не являются частью
общего профиля и должны задаваться на уровне доменного или tool-specific
применения.

Практическое правило уровней:

1. task-side planning signals могут проецироваться в delivery/tool profiles;
2. executor matching и capability registry должны оставаться в корневом слое
   `methodology/` или во внешнем registry-schema, а не в `delivery/linear`.

---

## 6. Anti-Patterns

Запрещено или нежелательно:

1. хранить measured executor capability прямо в task-state как свойство задачи;
2. использовать `Subjectivity` без фиксированного `Ref`;
3. считать creative mode общей меткой сложности без safety-hooks;
4. считать `Doubt` единственным основанием решения без evidence и gates;
5. смешивать planned alternatives и ветку `inapplicable`;
6. подменять evaluate-based creativity intake-label без runtime confirmation.

---

## 7. Normative References

- [MMCF-Canonical](../core/MMCF-Canonical.md)
- [MMCF-Creativity-and-Variability-Profile](../domain/MMCF-Creativity-and-Variability-Profile.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Executor-Matching-and-Capability-Registry-Profile](./MMCF-Executor-Matching-and-Capability-Registry-Profile.md)
- [CDM Creative ChangeFlow Canonical](../../../fcdm-core/theory/cdm/Specifications/System/CreativeChangeFlow-Canonical.md)
- [CDM Creative ChangeFlow Model Core](../../../fcdm-core/theory/cdm/Specifications/System/CreativeChangeFlow-Model-Core.md)
- [CDM MetaChangeFlow Model Core](../../../fcdm-core/theory/cdm/Specifications/System/MetaChangeFlow-Model-Core.md)
- [CDM Doubt Canonical](../../../fcdm-core/theory/cdm/Specifications/System/Doubt-Canonical.md)
- [CDM Subjectivity Canonical](../../../fcdm-core/theory/cdm/Specifications/System/Subjectivity-Canonical.md)
- [CDM Subjectivity Reference Agent Profile](../../../fcdm-core/theory/cdm/Specifications/System/Subjectivity-Reference-Agent-Profile.md)
- [MMCF-Delivery-Linear-Planning-Profile](../delivery/MMCF-Delivery-Linear-Planning-Profile.md)
