---
title: "MMCF: Executor Matching and Capability Registry Profile"
date: 2026-03-12
tags: [MMCF, executor, matching, capability, registry, profile]
status: profile-draft
---

# MMCF: Executor Matching and Capability Registry Profile

## 1. Scope

Этот документ задает будущий методологический профиль для:

1. подбора исполнителей;
2. capability-регистров;
3. matching между work units и исполнителями;
4. ранжирования сотрудников и AI-агентов;
5. аудируемого обоснования назначения.

Профиль намеренно расположен в корневом слое `methodology/`, а не в
`methodology/delivery/`, потому что он:

1. не привязан к одному tool-profile;
2. применим не только к Linear;
3. пересекает delivery, delegation, staffing и automation;
4. использует внешние evidence-backed capability records, а не runtime-state задачи.

---

## 2. Boundary

В минимальной delivery-схеме task issue хранит:

1. параметры самого потока;
2. assignee;
3. при необходимости короткую assignment note.

Task issue не хранит:

1. measured capability values исполнителя;
2. ranking candidates;
3. doubt/subjectivity bands как свойства задачи;
4. внутреннюю математику matching-механизма.

---

## 3. Planned contents

В следующей итерации этот профиль должен определить как минимум:

1. registry schema для human и AI executors;
2. context scope и reference profile semantics;
3. freshness / version / evidence requirements;
4. matching rules against task-side planning signals;
5. ranking and justification format;
6. handoff back into task systems через `assignee` и краткий assignment note.

---

## 4. Current rule

До появления полного matching-profile действует упрощенное правило:

1. task-side planning хранится в delivery/tool profiles;
2. matching и capability registry остаются вне issue-state;
3. issue не должен хранить `Doubt`, `Subjectivity`, capability bands или candidate scores.

---

## 5. References

- [MMCF-Planning-Assignment-Capability-Boundary](./MMCF-Planning-Assignment-Capability-Boundary.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Delivery-Linear-Planning-Profile](./delivery/MMCF-Delivery-Linear-Planning-Profile.md)
