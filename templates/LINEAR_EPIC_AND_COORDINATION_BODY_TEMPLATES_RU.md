---
title: "MMCF: готовые Linear body templates для Epic и coordination issue"
date: 2026-03-13
tags: [MMCF, delivery, Linear, epic, coordination, template]
status: working-draft
---

# MMCF: готовые Linear body templates для Epic и coordination issue

## 1. Назначение

Этот документ дает два готовых шаблона для прямого копипаста в `Linear`:

1. `Epic` body template
2. coordination issue body template

Используйте их только после того, как work object уже правильно
классифицирован.

Не используйте эти шаблоны для:

1. `DeltaRegistry` entry
2. terminal `ChangeFlow`
3. `PTSubTask`

Важно:

1. `Workflow Bridge` c `Backlog = DeltaRegistry`, `Todo = pre-start CF`,
   `In Progress = active CF` относится только к terminal `ChangeFlow`;
2. `Epic` и coordination issue не должны притворяться terminal `CF`.

---

## 2. Epic body template

Используйте этот шаблон, если объект является child lifecycle contour и должен
нести несколько содержательных terminal `ChangeFlow`.

Title rule:

- `[EPIC] <contour>: <objective>`

Не пишите в title:

1. `[CF#N]`
2. phase names
3. terminal exits
4. active bottlenecks

Готовый шаблон:

```md
## Type
- Epic / child lifecycle contour, not a terminal `ChangeFlow`.

## Purpose
- 

## Why now
- 

## Exit condition
- 

## Scope
- 

## Non-goals
- 

## Coordination rule
- Use this epic as the parent contour for related terminal `ChangeFlow` issues.
- Do not run `collect/analyze/forecast/decide/implement/evaluate` directly on the epic as if it were one flow.

## Evidence / refs
- 
```

Короткие правила заполнения:

1. `Purpose` отвечает на вопрос, зачем существует весь contour, а не одна
   локальная задача.
2. `Exit condition` должен описывать критерий завершения или трансформации
   всего epic-level contour.
3. `Scope` и `Non-goals` держат границу contour и снижают ложное разрастание.
4. Если внутри ожидается только один честный `ChangeFlow`, это, скорее всего,
   не `Epic`.

---

## 3. Coordination issue body template

Используйте этот шаблон, если объект нужен как:

1. umbrella container
2. decision log
3. checklist
4. follow-up collector

Title rule:

- `<descriptive coordination title>`

Не пишите в title:

1. `[EPIC]`, если это не lifecycle contour
2. `[CF#N]`
3. phase names
4. terminal exits

Готовый шаблон:

```md
## Type
- Coordination issue / tool-level container, not a terminal `ChangeFlow`.

## Purpose
- 

## Why this is coordination
- 

## Rule of use
- Use this issue for coordination, review notes, decision log, and follow-up collection.
- Do not imitate one full `collect -> analyze -> forecast -> decide -> implement -> evaluate` flow here.

## Spin-out rule
- Create a separate terminal issue when one atomic `ChangeFlow` becomes clear.

## Current related flows
- 

## Open points / checklist
- 

## Evidence / refs
- 
```

Короткие правила заполнения:

1. `Why this is coordination` должен честно объяснять, почему объект нельзя
   закрыть одним `CF6`.
2. `Current related flows` нужен только если coordination issue действительно
   связывает уже materialized terminal flows.
3. `Open points / checklist` можно вести свободно, но это не заменяет terminal
   flows для атомарной работы.
4. Как только возникает одна четкая атомарная delta, ее нужно spin-out'ить в
   отдельный terminal issue.

---

## 4. Минимальные варианты

### 4.1 Минимальный Epic

```md
## Type
- Epic / child lifecycle contour, not a terminal `ChangeFlow`.

## Purpose
- 

## Why now
- 

## Exit condition
- 
```

### 4.2 Минимальный coordination issue

```md
## Type
- Coordination issue / tool-level container, not a terminal `ChangeFlow`.

## Purpose
- 

## Why this is coordination
- 

## Spin-out rule
- Create a separate terminal issue when one atomic `ChangeFlow` becomes clear.
```
