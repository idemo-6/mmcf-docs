---
title: "MMCF: профиль отображения канона во внешние системы"
date: 2026-03-15
tags: [MMCF, canonical, external-system, mapping, interoperability]
status: profile-draft
---

# MMCF: профиль отображения канона во внешние системы

## 1. Scope

Этот профиль фиксирует общее правило интеграции MMCF с внешними системами:

- task trackers;
- docs/workflow platforms;
- registries;
- custom operational bridges;
- будущими MMCF-native системами.

Профиль не описывает конкретный tool mapping. Он задает приоритеты и границы:

1. что первично при интеграции;
2. что допустимо упрощать;
3. где упрощение становится разрушением канонического различия;
4. когда bridge-слой обязан materialize'ить отдельный operational object.

---

## 2. Canonical Primacy Principle

В MMCF каноническая модель первична по отношению к любой внешней системе.

Если внешняя система не способна выразить каноническую структуру напрямую,
адаптироваться должны:

1. bridge-layer;
2. tool mapping;
3. operational representation;
4. выбор системы или класс ее применимости.

Канон не должен редактироваться под ограничения UI, issue model, status model,
field model или comment model внешнего инструмента.

---

## 3. Mapping compatibility rule

Отображение внешней системы совместимо с MMCF только если оно сохраняет
релевантные для канона различия.

Допустимы три режима:

1. `full-fidelity mapping`
   Внешняя система или bridge-слой сохраняют все materially relevant различия.
2. `declared partial mapping`
   Система сохраняет только часть различий, а границы потерь объявлены явно.
3. `unsupported mapping`
   Система не способна выразить необходимое различие без разрушения канона.

Нежелательно притворяться `full-fidelity mapping`, когда фактически используется
скрытая редукция.

---

## 4. Principle of irreducible canonical distinctions

Если каноническое различие materially влияет на:

1. интерпретацию процесса;
2. runtime-наблюдаемость;
3. ответственность;
4. verdict/result;
5. bottleneck analysis;

то это различие нельзя схлопывать в:

1. поле;
2. статус;
3. комментарий;
4. декоративную метку;
5. неструктурированный текст;

если такое схлопывание уничтожает это различие как операционно наблюдаемую ось.

Следовательно, проблема не формулируется как
"как подогнать канон под объектную модель инструмента",
а как
"какая проекция или дополнительный operational object нужна, чтобы не потерять
канонически значимое различие".

---

## 5. Diagnostic case for explicit materialization

Следующие признаки не являются самим фундаментальным принципом, но служат
диагностикой того, что перед нами, вероятно, канонически нередуцируемый случай.

Если нечто:

1. имеет самостоятельный runtime;
2. не совпадает с phase state другого объекта;
3. имеет собственный результат или verdict;
4. может быть bottleneck отдельно от parent process;
5. требует отдельной наблюдаемости и ответственности;

то tool mapping не должен без проверки редуцировать это к атрибуту parent
object.

В таком случае следует рассмотреть:

1. secondary state axis;
2. explicit child object;
3. linked runtime entity;
4. иной bridge-механизм, который сохраняет различие.

---

## 6. What may be reduced and what may not

Допустимо упрощать:

1. UI presentation;
2. naming;
3. local workflow hints;
4. storage shape;
5. verbosity trace по умолчанию.

Недопустимо упрощать:

1. различие между parent process и отдельным runtime-переходом;
2. различие между phase state и transition state;
3. различие между execution bottleneck и gateway bottleneck;
4. различие между claim-bearing acceptance и обычным handoff;
5. различие между отдельным operational object и простым metadata field.

---

## 7. External-system obligations

Если внешний инструмент ограничен, у интегратора есть только три корректных
хода:

1. materialize'ить bridge-object или secondary axis;
2. явно объявить mapping частичным;
3. признать данный класс сущностей unsupported для этого инструмента.

Некорректный ход один:

1. молча уничтожить каноническое различие ради удобства существующей object
   model.

---

## 8. MMCF and external methodologies

`MMCF` не позиционируется как замена существующим методикам и инструментам:

1. `Scrum`
2. `Agile`
3. `Kanban`
4. `Linear`
5. иным delivery / workflow systems

Они трактуются как внешние operational systems, которые:

1. организуют cadence, board model, ritual layer и execution surface;
2. могут materialize'ить канонические сущности MMCF;
3. могут быть дополнены и дисциплинированы MMCF;
4. не задают канонический смысл сущностей MMCF.

Следовательно:

1. `MMCF` выступает более фундаментальным semantic layer;
2. внешняя методика или tool не переопределяет канон;
3. совместимость достигается через profile, mapping и bridge-layer;
4. если методика или tool теряют канонически значимое различие, это ограничение
   external system, а не основание переписывать канон.

В этом смысле термин `external system` означает не "чужой" или "ошибочный"
контур, а внешний относительно канонического слоя реализации.

---

## 9. Example: `PhaseTransition`

`PhaseTransition` является типовым примером, на котором этот принцип виден
наиболее отчетливо.

Если `PT` сводится только к:

1. phase status;
2. parent issue field;
3. comment;
4. label;

теряется различие между:

1. завершением фазы;
2. переходом между фазами;
3. gateway bottleneck;
4. phase bottleneck.

Поэтому в external systems типа `Linear` для materially significant `PT`
допустимо и желательно explicit materialization в отдельный operational object,
например `PTSubTask`.

При этом:

1. `PTSubTask` не становится новым `ChangeFlow`;
2. `PTSubTask` не заменяет parent issue;
3. `PTSubTask` является лишь bridge-формой для сохранения канонического
   различия во внешнем tool model.

---

## 10. Existing tools vs native systems

### 10.1 Existing tools

В existing tools допустимы компромиссы представления, но не компромиссы
канонической интерпретации.

То есть допустимо:

1. хранить часть trace в body-block;
2. использовать child issue вместо first-class runtime entity;
3. вводить secondary state axis поверх parent object.

Но недопустимо объявлять два разных канонических процесса одним и тем же
status field только потому, что UI так проще.

### 10.2 MMCF-native systems

В native system bridge-компромиссы должны стремиться к схлопыванию в
first-class канонические сущности и runtime objects.

То, что в existing tools выглядит как subtask, body-block или overlay,
в native implementation должно становиться прямым выражением канона.

---

## 11. Anti-patterns

Запрещено или нежелательно:

1. переписывать канон под ограничения task tracker;
2. объявлять редукцию "просто деталями реализации", если она уничтожает
   наблюдаемое различие;
3. смешивать runtime differentials с cosmetic metadata;
4. скрывать partial mapping и называть его полной совместимостью;
5. материализовывать operationally самостоятельную сущность только как comment
   trail, если ей требуется отдельная ответственность и verdict.

---

## 12. References

- [MMCF-Canonical](../core/MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](../core/MMCF-Minimal-Working-Model.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Operational-Gateway-Scenario-Profile](./MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Phase-and-Transition-State-Separation-Profile](./MMCF-Phase-and-Transition-State-Separation-Profile.md)
- [MMCF-PT-Template-Catalog-Profile](./MMCF-PT-Template-Catalog-Profile.md)
- [MMCF-Delivery-INDEX](../delivery/MMCF-Delivery-INDEX.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](../delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
