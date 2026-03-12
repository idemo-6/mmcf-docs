---
title: "MMCF Delivery: профиль `PhaseTransition` и шлюзов"
date: 2026-03-12
tags: [MMCF, delivery, PhaseTransition, gateway, ChangeFlow]
status: working-draft
---

# MMCF Delivery: профиль `PhaseTransition` и шлюзов

## 1. Назначение

Этот документ определяет минимальный профиль для `CF-PhaseTransition`
и наблюдаемости шлюзов в Linear-подобных операционных системах.

Практическая цель проста: отделить время исполнения фазы от времени на
переход, передачу, согласование и принятие.

Общий MMCF-профиль operational scenarios для gateway-layer вынесен в:

- [MMCF-Operational-Gateway-Scenario-Profile](../MMCF-Operational-Gateway-Scenario-Profile.md)

---

## 2. Канонический принцип

В практике delivery завершение фазы и переход фазы не совпадают.

Переход `CFi => CF(i+1)` трактуется как отдельный процесс со своими:

1. исходом;
2. длительностью;
3. ответственностью;
4. типом отказа.

Это позволяет видеть, находится ли узкое место в исполнении фазы или в
передаче, согласовании либо принятии claim/evidence.

---

## 3. Каноническая и операционная классификация

Каноническая классификация `CF-PT` остается такой:

1. `unconditional`
2. `conditional`

Для практики delivery используются следующие операционные сценарии:

1. `event`
2. `approve`
3. `claim`
4. `approve+claim`

Правило отображения:

1. `event` обычно является `unconditional`;
2. `approve`, `claim` и `approve+claim` обычно являются `conditional`.

Операционные метки не заменяют каноническую классификацию.

---

## 4. Операционные сценарии

### 4.1 `event`

Простой переход после завершения фазы.

Типовой смысл:

1. результат предыдущей фазы передается напрямую;
2. явный этап согласования не требуется;
3. следующая фаза может начаться сразу.

### 4.2 `approve`

Переход требует явного подтверждения, финального согласования или принятия.

Типовые случаи:

1. принятие ревью;
2. согласование со стороны owner;
3. релизный шлюз или шлюз политики;
4. редакторское или управленческое согласование.

### 4.3 `claim`

Переход требует валидной связки claim/evidence/canon до старта следующей
фазы.

Типовые случаи:

1. научная документация, несущая claims;
2. канонические документы или документы выравнивания;
3. software- или documentation-задачи с явным результатом, несущим claims.

### 4.4 `approve+claim`

Переход требует одновременно:

1. явного approval;
2. валидного claim/evidence acceptance.

Это типично, когда результат, несущий claims, также должен пройти owner или
управленческое финальное согласование.

---

## 5. Минимальная трасса для v1

Для каждой явной gateway-trace фиксируйте как минимум:

1. `from_phase`
2. `to_phase`
3. `pt_class = unconditional | conditional`
4. `pt_scenario = event | approve | claim | approve+claim`
5. `result = true | false`
6. `t_start`
7. `t_end`
8. `input_ref` и `output_ref`
9. `approval_ref`, когда approval обязателен
10. `claim_ref` или `evidence_ref`, когда обязательна передача результата, несущего claims
11. `failure_reason`, когда `result=false`

Для v1:

1. `approve`, `claim`, `approve+claim` и неуспешные переходы должны
   логироваться явно;
2. простые успешные `event`-переходы могут оставаться неявными внутри
   изменения статуса.
3. для фильтрации на уровне issue в Linear активное узкое место перехода можно
   зеркалить метками вроде `AwaitingApproval`, `AwaitingClaim` или
   `GatewayFailure`.

---

## 6. Заметка об ответственности

Ответственность за фазу и ответственность за шлюз остаются раздельными:

1. owner фазы отвечает за результат фазы;
2. owner шлюза отвечает за доставку, принятие и валидность перехода.

Следовательно, длинная очередь согласования является узким местом уровня
`PhaseTransition`, а не автоматически отказом соседней фазы.

---

## 7. Использование профиля

Рекомендуемые значения по умолчанию по профилям delivery:

1. разработка ПО: по умолчанию `event`; `approve` для review/release/security gates;
   `claim` только для результатов, несущих claims;
2. документация: по умолчанию `event`; `approve` для editor/owner/canon gates;
   `claim` только там, где есть материал, несущий claims;
3. научная документация: часто `claim` или `approve+claim`, но только на
   тех переходах, которые реально несут результат с claims.

---

## 8. Ссылки

- [MMCF-Operational-Gateway-Scenario-Profile](../MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [CDM PhaseTransition Overview](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition_Overview.md)
- [CDM PhaseTransition-CF](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
