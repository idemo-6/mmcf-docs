---
title: "MMCF: краткое руководство по формированию задач и work objects"
date: 2026-03-12
tags: [MMCF, delivery, Linear, work-objects, quick-guide]
status: working-draft
---

# MMCF: краткое руководство по формированию задач и work objects

## 1. Зачем это нужно

Этот документ нужен для простого пользовательского ответа на вопрос:

- что в работе должно быть `Project`;
- что должно быть `Epic`;
- что должно быть terminal issue;
- а что вообще не надо пытаться вести как один `ChangeFlow`.

Документ опирается на:

- CDM `AppliedRules`;
- текущую ontology `MMCF delivery`;
- практический кейс `Linear`, где ошибка с `IAG-107` показала, что
  coordination / umbrella work item нельзя автоматически трактовать как
  normal terminal `ChangeFlow`.

---

## 2. Базовое правило

В `MMCF delivery` используйте такую лестницу:

1. `Project` — только для родительского контура с собственным `LifeCycle`.
2. `Epic` — только для дочернего контура с собственным `LifeCycle`.
3. terminal issue — только для одного полного `ChangeFlow`.
4. `PTSubTask` — только для нетривиального `PhaseTransition` внутри уже
   существующего terminal issue.
5. coordination issue — только для координации, чек-листа, decision log или
   сборки follow-up work; это не normal terminal `CF`.

Короткая формула:

- `Project = parent LifeCycle`
- `Epic = child LifeCycle`
- terminal issue = `one ChangeFlow`
- `PTSubTask = one explicit non-trivial PhaseTransition`
- coordination issue = tool-level container, а не канонический work unit

---

## 3. Быстрое дерево выбора

### 3.1 Когда нужен `Project`

Создавайте `Project`, только если существует верхнеуровневый контур, который:

1. имеет собственную границу;
2. живет во времени как отдельная система;
3. несет дочерние lifecycle-контуры;
4. не сводится к одной теме или одной пачке задач.

Если это просто тематическая область или крупная цель без отдельного
lifecycle-смысла, `Project` не нужен.

### 3.2 Когда нужен `Epic`

Создавайте `Epic`, только если объект проходит `RLC/CC` относительно `Project`:

1. у него есть своя граница;
2. у него есть собственный lifecycle-контур;
3. внутри него ожидается не один, а несколько содержательных `ChangeFlow`;
4. у него есть свой критерий завершения или трансформации.

Если это просто одна задача, одна правка или один проход — это не `Epic`.

### 3.3 Когда нужен terminal issue

Создавайте terminal issue, если работа является одним атомарным потоком:

`collect -> analyze -> forecast -> decide -> implement? -> evaluate`

Признаки правильного terminal issue:

1. у него один carrier contour;
2. у него одна базовая intent-линия;
3. у него один `CF6`;
4. его можно честно закрыть через `done`, `repeat`, `final` или `delayed`.

Если это так, перед вами normal terminal `ChangeFlow`.

### 3.4 Когда нужен coordination issue

Создавайте coordination issue, если объект:

1. собирает несколько разнородных решений в одном месте;
2. ведет чек-лист или журнал решений;
3. координирует follow-up flows;
4. нужен как umbrella container, а не как один атомарный проход;
5. не имеет одного честного `CF6`, который закрывает весь контур как один
   `ChangeFlow`.

Такой объект допустим как tool convenience object, но его не надо
притворять terminal issue.

### 3.5 Когда нужен `PTSubTask`

Создавайте `PTSubTask`, если внутри уже существующего terminal issue у вас есть
нетривиальный переход между фазами:

1. `approve`
2. `claim`
3. `approve+claim`
4. process-based handoff
5. retrying или failed transition
6. unusually long transition, который сам стал bottleneck

`PTSubTask` не является новым `ChangeFlow`.

Он нужен только затем, чтобы:

1. не терять `PT` внутри phase-oriented UI;
2. держать отдельный owner/assignee/history перехода;
3. не превращать сам переход в параметр фазы.

---

## 4. Простое правило по AppliedRules

### 4.1 `RLC/CC`

Сначала спросите:

- это отдельный lifecycle-узел?
- или это только процесс внутри родителя?

Если lifecycle-узел — это кандидат в `Project` или `Epic`.  
Если процесс внутри родителя — это кандидат в terminal issue.

### 4.2 `ROS`

Остановите декомпозицию там, где новый уровень уже не дает:

1. больше объяснительной силы;
2. больше управленческой пользы;
3. более устойчивую оценку результата.

То есть не каждая тема должна становиться `Epic`.

### 4.3 `SEC/OC`

Внешний контекст не делайте `Epic`.

Если объект:

1. влияет на допустимые траектории;
2. но не является внутренней подсистемой на выбранном уровне;

то это `Context`, а не новый уровень иерархии.

### 4.4 `Observer`

`C_obs`, `C_coord`, `C_meta` — это роли исполнения и управления, а не уровни
work breakdown.

Не создавайте `Epic` только потому, что появился новый owner, reviewer,
observer или meta-controller.

---

## 5. Практические anti-patterns

### 5.1 `Epic` как просто большая тема

Ошибка:

- "большая тема" автоматически становится `Epic`

Почему это плохо:

- иерархия раздувается;
- lifecycle-смысл теряется;
- `Epic` перестает означать child `LifeCycle`.

### 5.2 Один issue как контейнер для многих независимых решений

Ошибка:

- в одном issue живет длинный разношерстный чек-лист

Почему это плохо:

- невозможно честно провести один `CF`;
- `collect/analyze/forecast/decide` становятся декоративными;
- `CF6` не фиксирует единый результат.

### 5.3 Контекст как будто это внутренняя подсистема

Ошибка:

- `Platform`, `Policy`, `Research`, `Regulatory` или другой внешний контур
  превращается в `Epic`

Почему это плохо:

- смешивается внутренняя декомпозиция и внешний контекст;
- нарушается `SEC/OC`.

### 5.4 Coordination issue как будто это terminal `ChangeFlow`

Ошибка:

- umbrella / checklist / decision-log issue ведут как один полноценный `CF`

Почему это плохо:

- нет одного carrier contour;
- нет одного честного `CF6`;
- комментарии по фазам начинают имитировать поток, а не описывать реальный
  atom of change.

### 5.5 `PTSubTask` как будто это ещё один `ChangeFlow`

Ошибка:

- подзадача перехода трактуется как отдельный terminal issue

Почему это плохо:

- ломается атомарность parent `ChangeFlow`;
- `PT` начинает притворяться новым потоком;
- теряется различие между `Phase` и `PhaseTransition`.

---

## 6. Кейс `IAG-107`

`IAG-107` оказался полезным негативным примером.

Что произошло:

1. в одну задачу был положен разнородный рефакторинговый чек-лист;
2. поверх него попытались вести строгую фазовую семантику;
3. быстро выяснилось, что это не один `ChangeFlow`, а coordination container.

Правильный вывод:

- такая задача допустима как рабочий координационный объект;
- но она не должна притворяться normal terminal issue;
- фазовый трейс для нее перестает быть главным, а чек-лист и decision log
  выходят на первый план.

Именно из этого кейса и выросло данное руководство.

---

## 7. Правило по умолчанию, если сомневаешься

Если сомневаешься между несколькими уровнями:

1. не создавай новый `Epic` без явного lifecycle-смысла;
2. не создавай coordination issue, если работу можно честно провести как один
   terminal `CF`;
3. не делай контекст отдельным lifecycle-узлом;
4. не делай `PTSubTask`, если переход короткий и materially незначим;
5. лучше начать с terminal issue и только потом повышать уровень, если это
   реально нужно.

Практически это означает:

- `Epic` должен быть исключением, которое нужно обосновать;
- terminal issue — нормальный default для атомарной работы;
- `PTSubTask` — специальный tool object только для нетривиального перехода;
- coordination issue — осознанный tool-level container, а не скрытая форма
  плохо сформулированного `CF`.

---

## 8. Что читать дальше

Для строгой методологической базы см.:

- [MMCF-Delivery-AppliedRules-Integration-Profile](./methodology/delivery/MMCF-Delivery-AppliedRules-Integration-Profile.md)
- [MMCF-Delivery-Linear-Profile](./methodology/delivery/MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Operational-Work-Unit-Contract](./methodology/MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Phase-and-Transition-State-Separation-Profile](./methodology/MMCF-Phase-and-Transition-State-Separation-Profile.md)
- [CDM RLC-CC Profile](../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM ROS Profile](../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md)
- [CDM SEC-OC Profile](../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md)
- [CDM Observer Profile](../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md)

Для tool-level compatibility case см.:

- [MMCF_LINEAR_COMPATIBILITY_CASE](./MMCF_LINEAR_COMPATIBILITY_CASE.md)
- [MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE](./MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE.md)
