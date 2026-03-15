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

Общий принцип того, почему канонически самостоятельная сущность не должна
редуцироваться под object model внешнего инструмента, вынесен отдельно в:

- [MMCF-Canonical-to-External-System-Mapping-Profile](../operational/MMCF-Canonical-to-External-System-Mapping-Profile.md)

Общий MMCF-профиль operational scenarios для gateway-layer вынесен в:

- [MMCF-Operational-Gateway-Scenario-Profile](../operational/MMCF-Operational-Gateway-Scenario-Profile.md)

Методологическое разделение между `phase state` и `transition state`,
которое этот tool-profile обязан сохранять, вынесено отдельно в:

- [MMCF-Phase-and-Transition-State-Separation-Profile](../operational/MMCF-Phase-and-Transition-State-Separation-Profile.md)

Reusable шаблонный слой поверх канона `PT` и над explicit `PTSubTask`
вынесен отдельно в:

- [MMCF-PT-Template-Catalog-Profile](../operational/MMCF-PT-Template-Catalog-Profile.md)
- [MMCF-Delivery-PT-Template-Catalog-Profile](./MMCF-Delivery-PT-Template-Catalog-Profile.md)

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

Нормативно:

1. `PT` не должен растворяться в phase status;
2. phase state и transition state должны оставаться различимыми даже в
   phase-oriented UI;
3. tool mapping может быть упрощенным, но не должен скрывать сам факт
   отдельного runtime-перехода.

Это является частным случаем более общего правила MMCF:

1. канон не подгоняется под ограничения внешней системы;
2. если инструмент не выражает каноническое различие напрямую, bridge-layer
   должен materialize'ить это различие отдельно;
3. `PTSubTask` в этом профиле является именно такой bridge-формой, а не новой
   канонической сущностью.

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
4. `pt_form = inline | process-based`
5. `pt_scenario = event | approve | claim | approve+claim`
6. `result = true | false`
7. `t_start`
8. `t_end`
9. `input_ref` и `output_ref`
10. `approval_ref`, когда approval обязателен
11. `claim_ref` или `evidence_ref`, когда обязательна передача результата, несущего claims
12. `failure_reason`, когда `result=false`
13. `pt_failure_policy`, когда переход не `fail_fast`
14. `attempt_no`, когда зафиксирована не первая попытка
15. `policy_state = retrying | exhausted | completed`, когда policy materially влияет на ход перехода

Удерживайте три независимые оси:

1. `pt_class` — канонический класс перехода;
2. `pt_form` — operational form перехода;
3. `pt_scenario` — semantic scenario bottleneck.

Для v1:

1. `approve`, `claim`, `approve+claim` и неуспешные переходы должны
   логироваться явно;
2. простые успешные `event`-переходы могут оставаться неявными внутри
   изменения статуса.
3. для фильтрации на уровне issue в Linear активное узкое место перехода можно
   зеркалить метками вроде `AwaitingApproval`, `AwaitingClaim` или
   `GatewayFailure`.

### 5.1 Явный `PT`-объект в Linear-like systems

Для existing tools, где phase-oriented board плохо показывает `PT`, в этом
профиле допускается explicit transition object.

Рекомендуемая форма для `Linear`:

- `PTSubTask`

`PTSubTask` используется только для нетривиальных переходов и всегда является
дочерним объектом терминального `ChangeFlow` issue.

Нормативно:

1. `PTSubTask` не является terminal `ChangeFlow`;
2. `PTSubTask` не несет собственного `CF5` или `CF6` как отдельный поток;
3. parent issue остается единственным атомарным `ChangeFlow`;
4. закрытие `PTSubTask` лишь разрешает или фиксирует соответствующий переход.

### 5.2 Когда создавать `PTSubTask`

`PTSubTask` рекомендуется создавать только для materially significant
transitions:

1. `approve`
2. `claim`
3. `approve+claim`
4. process-based handoff
5. retrying / failed transitions
6. unusually long `event`, если переход сам стал bottleneck

Простой короткий успешный `event` по умолчанию остается implicit и не должен
раздувать board.

### 5.3 Что хранит `PTSubTask`

Минимально:

1. `from_phase`
2. `to_phase`
3. `pt_class`
4. `pt_form`
5. `pt_scenario`
6. `pt_runtime_state`
7. `pt_failure_policy`, если она не `fail_fast`
8. `attempt_no`
9. `approval_ref`, если нужен approve
10. `claim_ref` или `evidence_ref`, если переход claim-bearing
11. `TransitionOwner`
12. `TransitionExecutor`, если handoff process-based
13. `template_id`, если переход materialized через selected template
14. `template_family`, если нужен reusable audit trail

Преимущество этого mapping:

1. `PT` получает отдельный stack owner/assignee/history;
2. phase status parent issue не раздувается;
3. каноническое различие `Phase` и `PT` визуально сохраняется лучше, чем при
   хранении всего перехода только в body parent issue.

### 5.4 Статусы `PTSubTask`

`PTSubTask` не должен использовать phase statuses `collect/analyze/...`.

Рекомендуемая малая status model:

1. `planned`
2. `in_transition`
3. `awaiting_approval`
4. `awaiting_claim`
5. `retrying`
6. `closed`
7. `failed`

Если workspace не может или не хочет вводить отдельные PT-statuses, допустим
компромисс:

1. generic issue status;
2. `PT Runtime State` как поле или обязательный structured body block.

### 5.5 Retry policy в delivery

Если для перехода используется recovery policy, она живет внутри того же
`PT`, а не создает новый `ChangeFlow`.

Рекомендуемые значения:

1. `fail_fast`
2. `retry_n(n)`
3. `retry_until_deadline(deadline)`
4. `retry_until_condition(condition_ref)`

Для `retry_until_condition(condition_ref)` в delivery рекомендуется использовать
только внешне наблюдаемое condition с явным evidence ref и stop rule.

Нормативно:

1. отдельная неуспешная попытка внутри retry policy не обязана немедленно
   отправлять flow в `evaluate`;
2. пока policy не исчерпана, issue остается в статусе исходной фазы, а не
   переходит в следующую фазу;
3. только terminal failure после исчерпания policy фиксируется как `pt_result=false`
   и ведет текущий `ChangeFlow` в `CF6`.

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

Практически:

1. `fail_fast` остается значением по умолчанию;
2. retry policy имеет смысл только там, где неуспешный переход не должен
   автоматически завершать flow;
3. для `approve`- и `claim`-gates retry policy чаще выражается как retry по
   deadline или по условию, а не бесконечный loop.
4. для distributed `CF` нетривиальные переходы по умолчанию следует
   рассматривать по форме как `process-based`, даже если не каждый из них
   materialized в отдельный `PTSubTask`.
5. если переход управляется через selected template, parent issue должен
   хранить template binding, а `PTSubTask` — только runtime-relevant copy и
   actual state.

---

## 8. Ссылки

- [MMCF-Canonical-to-External-System-Mapping-Profile](../operational/MMCF-Canonical-to-External-System-Mapping-Profile.md)
- [MMCF-Operational-Gateway-Scenario-Profile](../operational/MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Phase-and-Transition-State-Separation-Profile](../operational/MMCF-Phase-and-Transition-State-Separation-Profile.md)
- [MMCF-PT-Template-Catalog-Profile](../operational/MMCF-PT-Template-Catalog-Profile.md)
- [MMCF-Delivery-PT-Template-Catalog-Profile](./MMCF-Delivery-PT-Template-Catalog-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Operational-Roles-and-Gateways](../operational/MMCF-Operational-Roles-and-Gateways.md)
- [CDM PhaseTransition Overview](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition_Overview.md)
- [CDM PhaseTransition-CF](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
