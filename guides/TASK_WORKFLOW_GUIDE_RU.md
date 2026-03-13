---
title: "MMCF: руководство по workflow задачи"
date: 2026-03-13
tags: [MMCF, delivery, task-workflow, Linear, Codex, guide]
status: working-draft
---

# MMCF: руководство по workflow задачи

## 1. Зачем это нужно

Этот guide нужен для простого практического ответа на вопрос:

- как вести одну terminal задачу как один `ChangeFlow`;
- как разделять работу внутри фазы и переход между фазами;
- как Codex и человек делят роли в workflow задачи;
- как не смешивать `continue_phase`, `retry_gate` и `repeat`.

Guide ориентирован на `Linear`-подобный workflow, где:

- `Codex` является исполнителем;
- человек остается `CFOwner`, наблюдателем, центром принятия решений и
  финальной оценки, если явно не делегировано иначе.

---

## 2. Базовая модель

Одна terminal issue равна одному `ChangeFlow`:

`collect -> analyze -> forecast -> decide -> implement? -> evaluate`

Нормативно:

1. `evaluate` обязателен.
2. Если `Applicable=false`, поток все равно проходит через `decide` и
   `evaluate`.
3. `repeat` создает новый sibling flow; старый issue не продолжается как тот
   же `CF`.

---

## 3. Роли по умолчанию

По умолчанию используйте такую модель:

1. `Codex` — исполнитель текущей фазы.
2. пользователь — `CFOwner`.
3. пользователь — `Decision authority`.
4. пользователь — `Evaluation authority`.
5. `assignee` в Linear не равен автоматически `CFOwner`.

Это дает простой operational default: Codex делает работу и предлагает
следующий шаг, а человек дает verdict там, где требуется authority.

---

## 4. Что означает каждая фаза

### `collect`

Вопрос фазы:

- что именно относится к задаче и какие материалы реально в контуре работы.

Типичный outcome:

- список файлов;
- релевантные документы;
- ограничения;
- неизвестные места и blockers.

### `analyze`

Вопрос фазы:

- что именно нужно сделать и по каким критериям задача будет считаться
  успешной.

Типичный outcome:

- точная постановка;
- ограничения;
- допущения;
- риски.

### `forecast`

Вопрос фазы:

- какие есть варианты решения и чем они отличаются.

Типичный outcome:

- варианты;
- trade-offs;
- рекомендация.

### `decide`

Вопрос фазы:

- какой вариант выбран и применим ли он.

Типичный outcome:

- выбранный вариант;
- `Applicable=true|false`;
- граница реализации.

### `implement`

Вопрос фазы:

- что именно было materially изменено.

Типичный outcome:

- список изменений;
- затронутые файлы;
- локальные проверки.

### `evaluate`

Вопрос фазы:

- каков фактический операционный результат потока.

Типичный outcome:

- проверки или review;
- `Applicable`;
- рекомендуемый `Result`;
- рекомендуемый terminal exit.

---

## 5. Gate layer

После завершения каждой фазы существует `PT` (`PhaseTransition`).

Это значит:

1. завершение фазы не равно автоматически переходу в следующую фазу;
2. `PT` может ждать approval;
3. `PT` может ретраиться;
4. `PT` может завершиться terminal failure.

Рекомендуемый operational default:

- `strict_gated`

Это означает:

1. Codex завершает фазу и пишет phase comment;
2. затем формируется `PT`;
3. затем authority дает verdict о переходе дальше.

Простой короткий успешный `event`-переход можно оставить implicit как comment
trace. Нетривиальный переход лучше materialize как explicit `PTSubTask`.

---

## 6. Комментарии по фазам

Каждая фаза должна оставлять структурированный comment.

Рекомендуемый стиль:

1. короткий;
2. operational;
3. без пустых заглушек;
4. с явным `Requested verdict` в конце.

Минимальные формы:

### `collect`

```md
## collect
- Scope/files:
- Relevant refs:
- Open unknowns:
- Blockers:
- Requested verdict: continue_collect | accept_phase
```

### `analyze`

```md
## analyze
- Task statement:
- Constraints:
- Assumptions:
- Risks:
- Requested verdict: continue_analyze | accept_phase
```

### `forecast`

```md
## forecast
- Option A:
- Option B:
- Recommendation:
- Tradeoffs:
- Requested verdict: continue_forecast | decide_option <id> | accept_phase
```

### `decide`

```md
## decide
- Chosen option:
- Applicable:
- Decision rationale:
- Implementation boundary:
- Requested verdict: continue_decide | accept_phase
```

### `implement`

```md
## implement
- Changes made:
- Files changed:
- Checks run:
- Open issues:
- Requested verdict: continue_implement | accept_phase
```

### `evaluate`

```md
## evaluate
- Checks / review:
- Evidence refs:
- Applicable:
- Recommended Result: +1 | 0 | -1
- Failure class:
- Recommended exit: done | repeat | final | delayed
- Requested verdict: set_result <...> | close_flow <...>
```

### `PT`

```md
## PT: <from_phase> -> <to_phase>
- Scenario:
- Runtime state:
- Attempt:
- Blockers:
- Evidence/refs:
- Requested action: pass_gate | retry_gate | fail_gate
```

---

## 7. Командная модель

Для live-workflow используйте такие команды:

### Состояние и policy

- `show_flow_state`
- `set_gate_mode strict_gated|fast_path`
- `set_next_gate event|approve|claim|approve_claim`
- `materialize_gate`

### Работа внутри фазы

- `work_phase`
- `submit_phase`
- `continue_phase`
- `revise_phase`

### Управление переходом

- `accept_phase`
- `open_gate`
- `pass_gate`
- `retry_gate`
- `fail_gate`

### Решение и закрытие

- `decide_option <id>`
- `set_applicability true|false`
- `set_result -1|0|+1`
- `close_flow done|repeat|final|delayed`
- `spawn_repeat`

Если пользователь пишет это же обычным языком, operational meaning должен
оставаться тем же.

---

## 8. Самые важные различия

### `continue_phase`

Означает:

- работа в текущей фазе еще продолжается.

Не означает:

- новый `CF`;
- backward status move;
- retry transition.

### `retry_gate`

Означает:

- retry внутри уже открытого `PT`.

Не означает:

- повтор самой фазы;
- новый `CF`.

### `repeat`

Означает:

- текущий поток закрыт;
- новый sibling `ChangeFlow` должен начаться сразу.

Не означает:

- "доделаем ту же задачу потом";
- "вернемся в ту же фазу";
- "переоткроем старый issue".

### `delayed`

Означает:

- немедленный следующий поток не нужен;
- возврат ожидается позже при явном условии.

Поздний возврат все равно создает новый flow.

---

## 9. Статусы в Linear

Идеальная MMCF-совместимая status model:

- `Queued`
- `Todo`
- `collect`
- `analyze`
- `forecast`
- `decide`
- `implement`
- `evaluate`
- `done`
- `repeat`
- `final`
- `delayed`

Для текущего `Linear` workspace используйте такой operational bridge:

1. `Backlog` = upstream `DeltaRegistry`;
2. `Todo` = visible pre-start alias для materialized terminal `CF`;
3. `In Progress` = active terminal flow work;
4. точную фазу внутри `In Progress` держите через explicit phase status, если
   он есть, иначе через comments/body trace.

Важно:

1. workflow status `Todo` не равен issue-side `Planning` block;
2. planning custom fields `Flow Mode / Variativity Target / Decide Policy`
   тоже не равны workflow status `Todo`.

Если workspace пока не использует даже такой bridge, допустим временный режим:

1. держать issue в generic `In Progress`;
2. фазу вести через comments и body trace;
3. после настройки статусов перейти к явной phase status model.

Это допустимо только как transitional practice.

---

## 10. Рекомендуемый минимальный loop

1. если issue в `Todo`, сначала решить, готов ли поток войти в активную
   фазовую работу;
2. если issue в `In Progress`, понять текущую фазу по status или trace;
3. открыть issue и понять текущую фазу;
4. выполнить `work_phase`;
5. написать `submit_phase`;
6. если работа не завершена, использовать `continue_phase`;
7. если фаза завершена, использовать `accept_phase`;
8. открыть и провести `PT`, если переход нетривиален;
9. в `decide` зафиксировать `decide_option` и `Applicable`;
10. в `evaluate` зафиксировать `Result` и выбрать terminal exit;
11. если нужен немедленный следующий проход, использовать `repeat` и создать
   новый sibling flow.

---

## 11. Что не делать

1. не пропускать `evaluate`;
2. не использовать “повтор фазы” как отдельный статусный loop;
3. не смешивать `phase state` и `PT state`;
4. не трактовать approval delay как автоматически failure предыдущей фазы;
5. не переоткрывать старый issue вместо нового `repeat` flow;
6. не сводить `CFOwner` к одному `assignee`.

---

## 12. Ссылки

- [WORK_OBJECT_FORMATION_QUICK_GUIDE_RU](./WORK_OBJECT_FORMATION_QUICK_GUIDE_RU.md)
- [VERSIONING_QUICK_GUIDE_RU](./VERSIONING_QUICK_GUIDE_RU.md)
- [methodology/delivery/MMCF-Delivery-Linear-Profile](../methodology/delivery/MMCF-Delivery-Linear-Profile.md)
- [methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract](../methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile](../methodology/delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
