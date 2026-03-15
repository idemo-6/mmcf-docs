# CDM/MMCF относительно Scrum, Kanban, OODA и других подходов

Язык: RU
Статус: рабочий черновик
Область: `mmcf-docs`
Обновлено: 2026-03-13

## Назначение

Этот документ нужен как короткая входная позиция:

1. что такое `CDM/MMCF` по отношению к привычным workflow-подходам;
2. почему `CDM/MMCF` не стоит трактовать как еще одну конкурирующую
   "методологию управления задачами";
3. как `CDM/MMCF` соотносится с `Scrum`, `Kanban`, `OODA`, `Agile`, `Lean`
   и `Linear`.

Для более детального позиционирования и adoption-логики см.:

- [POSITIONING_AND_ADOPTION_ROADMAP](./positioning/POSITIONING_AND_ADOPTION_ROADMAP.md)
- [MMCF_COMPATIBILITY_AND_ADOPTION_MODES](./positioning/MMCF_COMPATIBILITY_AND_ADOPTION_MODES.md)
- [MMCF_LINEAR_COMPATIBILITY_CASE](./cases/MMCF_LINEAR_COMPATIBILITY_CASE.md)
- [MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE](./cases/MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE.md)

## Короткий тезис

`CDM/MMCF` не заменяет `Scrum`, `Kanban`, `OODA` или другие привычные подходы.

Он задает более фундаментальный слой:

- семантики изменения;
- границ work unit;
- фазовой дисциплины;
- transition/gateway semantics;
- applicability/result;
- governance, traceability и closure.

Базовая вертикаль:

- `FROR` отвечает на вопрос, какие инварианты должны сохраняться;
- `CDM` отвечает на вопрос, что такое изменение в канонической форме;
- `MMCF` отвечает на вопрос, как этим изменением управлять operationally;
- привычные методы отвечают на вопрос, в каком рабочем режиме люди это делают;
- tool-layer вроде `Linear` отвечает на вопрос, где это materialize'ится
  операционно.

Короткая формула:

- `FROR` отвечает за инварианты;
- `CDM` отвечает за каноническую логику изменения;
- `MMCF` отвечает за applied-layer управления;
- привычные методы отвечают за режим совместной работы;
- tool-layer отвечает за operational surface.

## Слои модели

### 1. `CDM`

`CDM` является каноническим основанием.

Он задает инварианты:

- `LifeCycle`;
- `ChangeFlow`;
- `PhaseTransition`;
- `Applicable`;
- `Result`;
- `Context`;
- `Experience`.

Это не process-trick и не набор менеджерских ритуалов.
Это модель изменения как такового.

### 2. `MMCF`

`MMCF` является applied management/governance-слоем поверх `CDM`.

Он добавляет:

- pre-flow слой `Delta / DeltaRecord / DeltaRegistry / PromotionDecision`;
- operational phase discipline;
- явную логику gateways и authority;
- planning, versioning, claim-governance;
- delivery-мосты в реальные инструменты и execution contours.

### 3. Workflow-подходы

`Scrum`, `Kanban`, `OODA`, `Agile-like` и другие подходы здесь лучше понимать
как внешние operating profiles.

Они могут:

- задавать cadence;
- задавать привычный user-facing vocabulary;
- задавать ритуалы и организационные ожидания;
- задавать режим визуализации и приоритизации.

Но они не обязаны быть самым глубоким semantic layer.

## Отношение к системной динамике

Изменения действительно являются частью динамики системы, поэтому
`CDM/MMCF` можно рассматривать как рамку для управления системной динамикой в
той части, где система меняется через наблюдаемые и проводимые intervention-потоки.

Но есть важная граница.

`CDM/MMCF` сильнее всего в:

- структурировании изменений;
- управлении change-contours;
- разделении observed delta и materialized work;
- governance и closure;
- трассируемом human/agent execution.

Классическая `system dynamics` сильнее всего в:

- feedback loops;
- stocks and flows;
- delays;
- quantitative simulation;
- emergent nonlinear behavior.

Следовательно, `CDM/MMCF` не заменяет классическую `system dynamics`.
Он лучше описывает и дисциплинирует не саму симуляцию системы, а
управляемое проведение изменений в системе.

## Позиционирование относительно Agile

`Agile` задает ценностную рамку:

- адаптивность;
- быстрый feedback;
- инкрементальность;
- приоритет реальной доставки над бюрократией.

Но `Agile` намеренно не является строгой формальной моделью change semantics.

`CDM/MMCF` дополняет `Agile` там, где нужны:

- более точные границы work unit;
- формальная applicability/result discipline;
- строгая closure semantics;
- аудируемые decision и transition traces.

Поэтому корректная формула такая:

`Agile` не заменяется `CDM/MMCF`;  
`CDM/MMCF` дает `Agile` более строгий semantic и governance layer.

## Позиционирование относительно Scrum

`Scrum` силен как командный operating profile.

Он хорошо задает:

- роли;
- cadence;
- инкрементную организацию работы;
- регулярные точки синхронизации;
- понятный командный ритм.

Но `Scrum` обычно не задает достаточно строгую модель:

- различия между сырой наблюдаемой проблемой и materialized work;
- атомарного `ChangeFlow`;
- отдельной семантики `PhaseTransition`;
- applicability/result;
- terminal closure и repeat semantics.

Поэтому `Scrum` можно понимать как team-mode поверх `MMCF`.

Формула:

- не `MMCF versus Scrum`;
- а `Scrum` как совместимый operating profile поверх `CDM/MMCF`.

## Позиционирование относительно Kanban

`Kanban` силен как flow-management approach.

Он особенно хорош в:

- pull-логике;
- WIP-discipline;
- визуализации потока;
- queue policy;
- локальной оптимизации throughput.

Но `Kanban` сам по себе обычно слабее формализует:

- что именно проходит через поток;
- где еще `Delta`, а где уже materialized work unit;
- где bottleneck относится к phase work, а где к `PT/gateway`;
- когда нужен новый flow, а не продолжение старого;
- что именно означает operational closure.

Поэтому `Kanban` хорошо ложится как flow-profile поверх `DeltaRegistry`,
`ChangeFlow` и `PT`-логики `MMCF`.

## Позиционирование относительно OODA

`OODA` сильна как компактная модель decision loop:

- `Observe`
- `Orient`
- `Decide`
- `Act`

В этом смысле `OODA` очень полезна как высокоуровневая phase-интуиция.

Но для управляемой operational execution сложных изменений она обычно слишком
сжата.

Ей часто не хватает явного различения:

- pre-flow delta layer;
- `forecast` и `decide`;
- `implement` и `evaluate`;
- phase state и transition state;
- terminal exits вроде `done/repeat/final/delayed`.

Поэтому `OODA` лучше понимать не как конкурента, а как возможную compressed
phase projection поверх более детальной `CF-6` логики.

## Позиционирование относительно Lean

`Lean` силен как рамка устранения потерь, ускорения потока и повышения
ценности.

Но `Lean` не стремится быть строгой онтологией change units и gateway logic.

`CDM/MMCF` может усиливать `Lean` там, где нужно:

- формально отделить observation от execution;
- удержать atomic work unit;
- сделать evaluate и operational result явными;
- различать correction, retry, repeat и terminal closure.

## Почему это хорошо легло на Linear

Кейс `Linear` особенно важен потому, что `MMCF` лег в реальную agile-like
систему в основном через техническое отображение, а не через смысловую
деформацию модели.

Сейчас это видно по таким mapping'ам:

- `Backlog` как upstream `DeltaRegistry`;
- `Planning` как collapsed pre-start materialized `ChangeFlow`;
- `In Progress` как active `ChangeFlow`;
- comments, child issues, labels и fields как носители phase/PT/evaluate trace.

Это важно потому, что показывает:

1. `MMCF` не требует сначала построить новую платформу;
2. `MMCF` может лечь под существующий agile/tool workflow;
3. привычный для команды режим может сохраняться, а глубина semantics и
   governance при этом возрастает.

Именно поэтому текущий `Linear`-кейс лучше трактовать не как "MMCF стала
еще одной досочной методологией", а как proof of compatibility более глубокого
change/governance layer с реальным инструментальным контуром.

## Практическая формула внедрения

Если объяснять команде без методологической перегрузки, формула должна быть
такой:

1. не нужно отказываться от привычного `Scrum/Kanban/Agile/OODA`-языка;
2. не нужно сначала перестраивать все ритуалы;
3. `CDM/MMCF` можно вводить как более глубокий слой semantics, traceability и
   governance;
4. сначала достаточно compatibility mode, затем augmentation, затем governed
   mode.

Это уже согласуется с действующим позиционированием проекта:

- не replacement-first;
- а compatibility-first;
- затем augmentation;
- затем governed/native usage.

## Самая короткая формула

Если нужно объяснить совсем кратко:

`Scrum`, `Kanban`, `OODA` и похожие подходы задают режим работы.  
`CDM/MMCF` задает модель того, чем именно этот режим управляет.

Или еще короче:

`CDM/MMCF` — это не еще одна методология задач.  
Это более фундаментальный слой семантики изменения, governance и traceability.
