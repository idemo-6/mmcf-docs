---
title: "MMCF: готовый Linear body template для PTSubTask"
date: 2026-03-13
tags: [MMCF, delivery, Linear, PTSubTask, PhaseTransition, template]
status: working-draft
---

# MMCF: готовый Linear body template для PTSubTask

## 1. Назначение

Этот шаблон нужен для прямого копипаста в `Linear` issue body, когда
нетривиальный `PhaseTransition` materialize'ится как explicit `PTSubTask`.

Используйте его только если:

1. parent issue уже является terminal `ChangeFlow`;
2. переход materially significant;
3. переход не должен оставаться только comment/body note на parent issue.

Типовые случаи:

1. `approve`
2. `claim`
3. `approve+claim`
4. process-based handoff
5. retrying / failed transition
6. unusually long transition bottleneck

Важно:

1. `PTSubTask` не является terminal `ChangeFlow`;
2. `PTSubTask` не использует phase status model `collect/analyze/...`;
3. `Workflow Bridge` вида `Backlog = DeltaRegistry`, `Planning = pre-start CF`,
   `In Progress = active CF` относится к parent terminal flow, а не к
   `PTSubTask`.

---

## 2. Готовый шаблон

```md
## PT Runtime
- Parent flow:
- Template id:
- Template family:
- From phase:
- To phase:
- PT class:
- PT form:
- PT scenario:
- PT runtime state:
- Failure policy:
- Attempt no:
- TransitionOwner:
- TransitionExecutor:
- Approval authority:
- Claim authority:
- Approval refs:
- Claim refs / evidence refs:
- Failure reason:
- Refs:
```

Suggested title:

`PT: <from_phase> -> <to_phase> / <template_id or pt_scenario>`

Примеры:

1. `PT: forecast -> decide / committee-approval`
2. `PT: analyze -> forecast / claim`
3. `PT: implement -> evaluate / ai-authority-approval`

---

## 3. Короткие правила заполнения

1. `Parent flow` должен ссылаться на parent terminal issue.
2. `Template id` и `Template family` заполняйте только если parent issue уже
   хранит selected PT template binding.
3. `PT class`, `PT form` и `PT scenario` не смешивайте:
   - `PT class` = `unconditional | conditional`
   - `PT form` = `inline | process-based`
   - `PT scenario` = `event | approve | claim | approve+claim`
4. Для short successful `event` обычно не нужен `PTSubTask`.
5. `TransitionOwner` и `TransitionExecutor` не заменяют `CFOwner` parent flow.
6. `Approval refs` и `Claim refs / evidence refs` заполняйте только когда
   переход реально approve-bearing или claim-bearing.
7. `Failure reason` нужен только если переход failed или retrying по policy.

---

## 4. Минимальный вариант

Если нужен совсем короткий body:

```md
## PT Runtime
- Parent flow:
- From phase:
- To phase:
- PT class:
- PT scenario:
- PT runtime state:
- TransitionOwner:
- Refs:
```

---

## 5. Что не писать в PTSubTask

Не переносите сюда как будто это второй flow:

1. `Base intent`
2. `Expected delta`
3. `Success criteria` всего parent flow
4. полный `Roles / Authority` block parent issue
5. `CF6 Summary`
6. phase comments `collect/analyze/forecast/decide/implement/evaluate`

Это все остается на parent terminal issue.
