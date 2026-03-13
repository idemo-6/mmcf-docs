---
title: "MMCF: готовый Linear body template для terminal ChangeFlow"
date: 2026-03-13
tags: [MMCF, delivery, Linear, terminal-issue, template]
status: working-draft
---

# MMCF: готовый Linear body template для terminal ChangeFlow

## 1. Назначение

Этот шаблон нужен для прямого копипаста в `Linear` issue body.

Используйте его только для terminal `ChangeFlow`, который уже:

1. прошел `PromotionDecision=promote`;
2. не является `DeltaRegistry` entry;
3. не является coordination issue;
4. не является `PTSubTask`.

Текущий `Linear` workspace bridge:

1. `Backlog` = upstream `DeltaRegistry`
2. `Todo` = pre-start materialized `ChangeFlow`
3. `In Progress` = active phase work

Важно:

1. workflow status `Todo` не равен body-блоку `Planning`;
2. planning custom fields тоже не равны workflow status `Todo`.

---

## 2. Готовый шаблон

```md
## Workflow Bridge
- Linear status bridge: `Backlog = DeltaRegistry`, `Todo = pre-start CF`, `In Progress = active CF`.

## Canon
- Parent Epic:
- Base task ref:
- Carrier entity:
- CF index:
- Pre-CF version:
- Version source ref:
- Base intent:
- Active context(s):
- LC phase snapshot:

## Delivery
- Work domain:
- Artifact type:
- Claim mode:
- Expected delta:
- Success criteria:
- Stop criteria:

## Roles / Authority
- CFOwner:
- Current assignee / active executor:
- Current CFPhaseOwner:
- TransitionOwner(s):
- Decision authority:
- Commit authority:
- Evaluation authority:
- Gateway approval authority:
- Claim authority:
- Risk class:
- Independence note:

## Planning
- Flow mode:
- Variativity target:
- Decide policy:
- max_decide_delay:
- forced_collapse_trigger:
- resource_cap:
- Planning rationale:

## PT / Gateways
- Default PT scenario: event
- Transition templates:
- Non-trivial transitions:
- PT failure policy:
- Negative result policy:
- Approval refs / owners:
- Claim refs / evidence refs:

## Evidence
- Main evidence refs:
- Related specs/docs:
- Constraints:
```

---

## 3. Короткие правила заполнения

1. Если `Work domain=meta-operational`, `Artifact type` не заполняйте
   искусственно.
2. Если planning `v1.1` еще не используется, блок `Planning` можно оставить,
   но заполнять только реально используемые поля.
3. Если все переходы простые, в `PT / Gateways` достаточно:
   - `Default PT scenario: event`
   - `Non-trivial transitions: none`
4. Если текущий issue создается для immediate active work, все равно body
   остается тем же; меняется только workflow status.
5. Не пишите в title фазу, exit или активный bottleneck.

---

## 4. Минимальный вариант

Если нужен совсем короткий body без planning-extension:

```md
## Workflow Bridge
- Linear status bridge: `Backlog = DeltaRegistry`, `Todo = pre-start CF`, `In Progress = active CF`.

## Canon
- Parent Epic:
- Base task ref:
- Carrier entity:
- Base intent:
- Active context(s):
- LC phase snapshot:

## Delivery
- Work domain:
- Claim mode:
- Expected delta:
- Success criteria:
- Stop criteria:

## Roles / Authority
- CFOwner:
- Current assignee / active executor:
- Decision authority:
- Evaluation authority:
- Risk class:

## PT / Gateways
- Default PT scenario: event
- Non-trivial transitions:

## Evidence
- Main evidence refs:
- Related specs/docs:
- Constraints:
```
