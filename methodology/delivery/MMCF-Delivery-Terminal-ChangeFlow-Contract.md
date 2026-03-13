---
title: "MMCF Delivery: контракт терминального `ChangeFlow`"
date: 2026-03-12
tags: [MMCF, delivery, terminal-issue, ChangeFlow, contract]
status: working-draft
---

# MMCF Delivery: контракт терминального `ChangeFlow`

## 1. Назначение

Этот документ определяет контракт для терминальной задачи, трактуемой как
один `ChangeFlow`.

Контракт применяется к работе по ПО, документации и научной документации.

Общий MMCF-смысл operational work unit и terminal exit semantics вынесен в:

- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](../MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)
- [MMCF-Operational-Work-Unit-Contract](../MMCF-Operational-Work-Unit-Contract.md)

---

## 2. Определение

Терминальная задача представляет собой один операционный проход:

`CF1 -> CF2 -> CF3 -> CF4 -> CF5? -> CF6`

где:

- `CF5` исполняется только если `Applicable=true`;
- `CF6` всегда обязателен перед закрытием.

Issue не является узлом `LifeCycle`.
Это процесс внутри родительского `Epic`.

### 2.1 Upstream pre-flow boundary

Терминальная задача создается только после того, как соответствующая `Delta`
прошла promotion из `DeltaRegistry` в materialized `ChangeFlow`.

Нормативно:

1. raw `DeltaRecord` не является terminal issue;
2. `Expected delta` в terminal issue описывает уже promoted change line, а не
   произвольную сырую запись из `DeltaRegistry`;
3. рекомендуемый pre-start status для созданного terminal issue — `Queued`;
4. прямое создание в `Todo` допустимо только когда queue-layer намеренно
   пропускается как локальное упрощение.

Для текущего `Linear` workspace допустим такой bridge:

1. `Backlog` читается как upstream `DeltaRegistry`;
2. materialized terminal issue создается в `Planning` как collapsed alias
   pre-start состояний `Queued/Todo`;
3. active phase work начинается в `In Progress`.

---

## 3. Входные данные и шаблон body

### 3.1 Обязательные метаданные issue на intake

При создании терминальной задачи задайте как минимум:

1. title согласно правилам именования;
2. `Parent Epic`;
3. `LC Phase Snapshot`;
4. `Work Domain`;
5. `Artifact Type`, если `Work Domain=artifact`;
6. `Claim Mode`.

Если применяется `Level 2` role profile или поток не является явно
`low-risk`, дополнительно зафиксируйте:

1. `CFOwner`
2. `Risk Class`
3. `Decision Authority`
4. `Evaluation Authority`

`Result` не является полем intake. Он выставляется только в `evaluate`.

Если задача привязана к versioned carrier entity, дополнительно задайте:

1. `Carrier Entity Id`
2. `CF Index`
3. `Pre-CF Version`, когда он уже известен

Если workspace использует planning `v1.1`, дополнительно задайте:

1. `Flow Mode`
2. `Variativity Target`
3. `Decide Policy`

Если workspace использует явный pre-start queue:

1. создавайте issue в `Queued` как в дефолтном состоянии;
2. переводите в `Todo`, когда поток уже допускается к активному старту.

Если workspace использует текущий `Linear` bridge вместо явного `Queued/Todo`:

1. не создавайте terminal issue в `Backlog`;
2. создавайте issue в `Planning` как default pre-start alias;
3. переводите в `In Progress`, когда поток реально входит в active phase work.

### 3.2 Обязательные поля body

Каждый body терминальной задачи должен содержать как минимум:

1. `Parent Epic`
2. `Base task ref`
3. `Carrier entity`
4. `CF index`, если carrier entity versioned
5. `Pre-CF version`, если carrier entity versioned
6. `Base intent`
7. `Active context(s)`
8. `LC phase snapshot`
9. `Work domain`
10. `Artifact type`, если `Work Domain=artifact`
11. `Expected delta`
12. `Success criteria`
13. `Stop criteria`
14. `Evidence refs`
15. `CFOwner`
16. `Current assignee / active executor`
17. `Risk class`

Рекомендуемый шаблон body задачи:

```md
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

Для текущего `Linear` workspace можно добавить в шаблон короткую operational note:

```md
## Workflow Bridge
- Linear status bridge: `Backlog = DeltaRegistry`, `Planning = pre-start CF`, `In Progress = active CF`.
```

Эта note опциональна и не заменяет ни workflow status, ни planning fields.

### 3.3 Руководство по `PT / Gateways`

Блок `PT / Gateways` используется для предварительного объявления
нетривиальных переходов.

Если все ожидаемые успешные переходы являются простыми `event`-переходами,
секция может оставаться минимальной:

- `Default PT scenario: event`
- `Non-trivial transitions: none`

Если ожидается approval или передача результата, несущего claims, укажите это явно, например:

- `forecast -> decide = committee-approval`
- `implement -> evaluate = ai-authority-approval`
- `forecast => decide: approve`
- `implement => evaluate: approve`
- `analyze => forecast: claim`
- `PT failure policy: default=fail_fast; implement=>evaluate=retry_n(2)`

Это дает потоку явный план переходов до появления узких мест.

Если используется reusable PT template catalog, template binding следует
указывать именно на конкретном `from_phase -> to_phase`, а не как абстрактную
свойство issue вообще.

### 3.4 Руководство по `Planning`

Блок `Planning` описывает только task-side planning contour задачи.

Он может включать:

1. execution mode;
2. planned alternative width;
3. decision-collapse policy;
4. safety-hooks creative/delayed mode;
5. краткое обоснование planning допущений, включая при необходимости note о
   риске reframing/escalation.

Он не должен включать:

1. executor capability values;
2. `Doubt` или `Subjectivity` bands как свойства задачи;
3. candidate ranking или suitability scores.

Важно:

- status alias `Planning` в текущем `Linear` bridge не равен body-блоку
  `Planning`.

### 3.4 `assignee` и role trace

В tool-layer `assignee` отвечает только на вопрос, кто держит ближайшее
операционное действие.

Нормативно:

1. `assignee` не является каноническим синонимом `CFOwner`;
2. `CFOwner` обязан быть различим как функция continuity/closure потока, даже
   если совпадает с тем же агентом;
3. для normal, high-risk, claim-bearing или governance-heavy flow body должен
   содержать явный блок `Roles / Authority`;
4. при смене `CFOwner` или materially significant authority map должен
   появляться handoff/update trace в комментариях issue.

### 3.5 `Work Domain` vs `Artifact Type`

`Work Domain` и `Artifact Type` в этом профиле являются разными осями.

1. `Work Domain` отвечает на вопрос, относится ли поток к artifact-bearing
   delivery или к meta-operational работе.
2. `Artifact Type` отвечает только на вопрос, какой основной тип carrier
   артефакта меняется.

Нормативно:

1. `Work Domain` обязателен для каждой terminal issue;
2. `Artifact Type` обязателен только если `Work Domain=artifact`;
3. для `Work Domain=meta-operational` не нужно искусственно выбирать
   `Artifact Type`, если поток не меняет основной carrier artifact.

---

## 4. Фазовая дисциплина

Обязательный порядок фаз:

- `collect`
- `analyze`
- `forecast`
- `decide`
- `implement`, когда применимо
- `evaluate`

Если неприменимость обнаружена во время `analyze` или `forecast`, issue все
равно обязан пройти через:

- `decide` с `Applicable=false`
- `evaluate` с `Result=0`

Только после этого issue может выйти через `final` или `delayed`.

Завершение фазы само по себе не означает закрытую передачу в следующую фазу.
Там, где требуются approval, claim/evidence acceptance или явное отслеживание
передачи, переход должен следовать профилю:

- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)

Attempt-level failures внутри retry policy не обязаны немедленно переводить
flow в `evaluate`. Только terminal failure перехода завершает текущий
`ChangeFlow` через `CF6`.

---

## 5. Контракт закрытия `CF6`

Фаза `evaluate` должна завершаться структурированным комментарием.

Комментарий `CF6` является итоговой сводкой, а не полным журналом phase или
gateway. Явные gateway traces остаются отдельными, но `CF6` обязан указать,
какой слой отказа или слой перехода материально определил исход потока.

Минимальный шаблон:

```md
## CF6 Summary
- Applicable: true | false
- Result: +1 | 0 | -1
- Failure class: none | phase-logic | applicability | gateway | mixed
- Active context:
- LC phase snapshot:
- Evidence refs:
- Post-CF version:
- Version outcome note:
- PT trace refs:
- PT summary:
- Exit decision: done | repeat | final | delayed
- Closure reason: success | inapplicable | failed | mixed
- Zero class: AF_sem | AF_epi | AF_sto | mixed
- Repeat reason:
- Next CF issue:
- Return condition:
- Review date / trigger:
- Summary:
- CFOwner at closure:
- Authority conflict refs:
- Role handoff refs:
```

`Pre-CF version` означает унаследованный snapshot до входа в текущий `CF#N`.
Он может содержать предыдущий `cf` сегмент относительно текущего `CF index`,
и это нормально.

### 5.1 Руководство по `CF6`

1. `Failure class` объясняет, какой слой в первую очередь определил terminal
   outcome:
2. если `CF5` породил evidence негативного исхода, но closure не предзадан,
   `PT(CF5->CF6)` может быть `approve` или `approve+claim`, а authority verdict
   выбирает `repeat`, `final` или `delayed`;
3. `Closure reason` объясняет, почему выбран именно этот exit:
   `success`, `inapplicable`, `failed` или `mixed`;
4. `Zero class` заполняется только когда `Result=0`.
   - `phase-logic`
   - `applicability`
   - `gateway`
   - `mixed`
   - `none` для прямолинейного успешного завершения
2. `PT trace refs` указывают на явные gateway traces, когда:
   - требовался approval;
   - требовалась передача результата, несущего claims;
   - terminal failure перехода завершился неуспешно;
   - нетривиальный переход создал основное узкое место
3. `PT summary` является короткой итоговой заметкой, например:
   - `implicit event transitions only`
   - `implement => evaluate approve completed after owner review`
   - `forecast => decide approval stalled for 9d`
   - `implement => evaluate failed as gateway; handoff not accepted`
   - `implement => evaluate retried twice, then accepted on third attempt`
4. `Zero class` заполняется только когда `Result=0`.
5. `Repeat reason` и `Next CF issue` обязательны только для `repeat`.
6. `Return condition` и `Review date / trigger` обязательны только для
   `delayed`.
7. если carrier entity versioned, `CF index` должен быть согласован с title
   `[CF#N]`;
8. `Pre-CF version` отражает snapshot до входа в текущий `CF` и потому может
   содержать предыдущий `cf` сегмент относительно текущего issue;
9. `Post-CF version`, когда она уже выведена, должна быть согласована с
   текущим `CF index`.
10. для `delivery/linear` новый material `Post-CF version` по умолчанию
    ожидается при `done` и `repeat`;
11. для `delivery/linear` при `final` и `delayed` по умолчанию используется
    `Version outcome note: no material version change`, если только carrier
    entity не изменилась materially и source of truth не вывел новый snapshot.
12. `CFOwner at closure` обязателен, если в ходе потока происходил handoff или
    если `CFOwner` не совпадает с последним `assignee`;
13. `Authority conflict refs` обязательны, если по потоку был зафиксирован
    `authority-conflict`.

---

## 6. Правила выхода

Terminal exit выбирается после `CF6` из трех независимых вопросов:

1. какой `Result` наблюдался;
2. требуется ли немедленно следующий `ChangeFlow`;
3. если не немедленно, ожидается ли будущий возврат или нет.

Исходы `PhaseTransition` влияют на это решение, но не заменяют его.
Узкое место на gateway или задержка approval сами по себе не являются terminal exit.
Attempt-level PT failures под активной retry policy также сами по себе не
являются terminal exit.

### 6.1 `done`

Значение по умолчанию:

- `Result=+1`
- следующий `CF` не требуется

По умолчанию допускается только когда:

1. `Applicable=true`
2. `Result=+1`
3. не требуется немедленный следующий `CF`
4. любой нетривиальный approval- или переход с результатом, несущим claims, необходимый
   для закрытия, уже успешно завершен

Не используйте `done`:

1. в значении "implementation finished but approval is still pending";
2. чтобы скрыть обязательное корректирующее продолжение;
3. чтобы закрыть поток с неразрешенным gateway failure.

### 6.2 `repeat`

Значение по умолчанию:

- текущий `CF` завершен
- следующий `CF` должен быть создан немедленно

Допустим, когда текущий поток завершен и новый соседний поток должен начаться
сейчас.

Типовые валидные комбинации:

1. `Applicable=true`, `Result=+1`, но более узкая следующая дельта уже
   известна;
2. `Applicable=true`, `Result=-1`, и `negative_result_policy` либо authority
   verdict на `PT(CF5->CF6)` выбирают немедленный corrective/compensating поток;
3. `Applicable=false`, `Result=0`, и задача немедленно переформулируется в
   новый поток с другой выполнимой дельтой.

PT-aware note:

1. если основным outcome был `GatewayFailure`, `repeat` является выходом по
   умолчанию только тогда, когда восстановление начинается сейчас в новом потоке;
2. `repeat` не является временной меткой "может быть позже".

Обязательно:

- `Repeat reason`
- `Next CF issue`

### 6.3 `final`

Значение по умолчанию:

- линия закрывается без немедленного или ожидаемого будущего продолжения

Используйте `final`, когда текущий поток закрывает линию без немедленного или
ожидаемого будущего продолжения.

Типовые валидные случаи:

1. `Applicable=false`, `Result=0`, и canonical или управленческое решение
   установило реальную неприменимость;
2. `Applicable=true`, `Result=-1`, и `negative_result_policy` либо authority
   verdict на `PT(CF5->CF6)` выбрали terminal stop;
3. approval или review по claims завершились стабильным решением "не продолжать
   эту линию";
4. текущая формулировка отклонена без планируемой reformulation.

PT-aware note:

1. failed approval или claim transition не означает автоматически `final`;
2. `final` корректен только тогда, когда исход transition или policy
   устанавливает terminal stop, а не просто задержку;
3. для `delivery/linear` `final` по умолчанию сопровождается
   `Version outcome note: no material version change` только для ветки
   `Result=0`; при `Result=-1` outcome note зависит от фактического material
   change.

Обязательно:

- причина финальной остановки

### 6.4 `delayed`

Значение по умолчанию:

- немедленный следующий поток не нужен
- возврат ожидается позже

Используйте `delayed`, когда текущий поток неприменим сейчас, но известен или
ожидается будущее условие корректного возврата.

Типовые валидные случаи:

1. `Applicable=false`, `Result=0`, и отсутствует заморозка вышестоящей
   спецификации;
2. `Applicable=true`, `Result=-1`, и `negative_result_policy` либо authority
   verdict на `PT(CF5->CF6)` выбрали deferred return;
3. ожидается управленческое решение;
4. не хватает evidence или завершения protocol;
5. недоступна зависимость, окружение или окно ресурсов.

PT-aware note:

1. само по себе длительное время approval недостаточно для `delayed`;
2. `delayed` корректен только тогда, когда поток закрывается потому, что
   следующий валидный ход зависит от более позднего условия, а не от
   немедленной корректирующей работы.
3. для `delivery/linear` `delayed` по умолчанию сопровождается
   `Version outcome note: no material version change` только для ветки
   `Result=0`; при `Result=-1` outcome note зависит от фактического material
   change.

Обязательно:

- `Return condition`
- `Review date / trigger`
- владелец возврата

### 6.5 Матрица решений

Используйте следующее отображение по умолчанию:

1. `Result=+1` и немедленный следующий `CF` не нужен -> `done`
2. `Result=+1` и нужен немедленный следующий `CF` -> `repeat`
3. `Result=-1` и `negative_result_policy` или authority verdict выбирают
   немедленный corrective/compensating `CF` -> `repeat`
4. `Result=-1` и `negative_result_policy` или authority verdict выбирают
   terminal stop -> `final`
5. `Result=-1` и `negative_result_policy` или authority verdict выбирают
   deferred return -> `delayed`
6. `Result=0` и есть немедленно reformulated `CF` -> `repeat`
7. `Result=0` и немедленного следующего `CF` нет, но ожидается будущий возврат ->
   `delayed`
8. `Result=0` и немедленного следующего `CF` нет, и возврат не планируется -> `final`

Нормативно:

1. `done` используется только для `Result=+1`;
2. при `Result=0` operationally играют только `repeat`, `final` и `delayed`;
3. при `Result=-1` ожидается, что `CF5` дает evidence, `PT(CF5->CF6)`
   разрешает exit через policy или authority verdict, а `CF6` фиксирует и
   `Result=-1`, и выбранный exit.

---

## 7. Правило цепочки повторов

Когда задача выходит через `repeat`:

1. текущий issue закрывается;
2. новый sibling issue создается под тем же `Epic`;
3. новый issue наследует тот же `Base task ref`;
4. новый issue сохраняет id предыдущего issue и delta для следующего прохода.

Рекомендуемая схема именования:

### 7.1 Форма title

Заголовки терминальных задач должны использовать стабильную форму contour/objective:

`<Contour>: <base objective> [CF#N]`

Где:

1. `Contour` является коротким стабильным именем родительского lifecycle
   contour или epic;
2. `<base objective>` является стабильной человекочитаемой целью этого потока;
3. `[CF#N]` идентифицирует конкретный `ChangeFlow` в цепочке повторов.

Рекомендуемая форма для repeat:

`<Contour>: <base objective> [CF#(N+1)] / <delta from previous CF>`

### 7.2 Правила naming

1. Title идентифицирует цель потока, а не текущую фазу.
2. Алиасы статусов вроде `collect`, `implement`, `evaluate` не должны
   попадать в title.
3. Сценарии `PhaseTransition`, например approval или узкие места по claims, не
   должны попадать в title.
4. `Result`, `Applicable`, `done/repeat/final/delayed` и labels активных
   blockers не должны попадать в title.
5. Префикс contour должен оставаться стабильным через sibling flows в том же epic.
6. Суффикс delta после `/` используется только тогда, когда он материально
   заостряет следующий повторный поток.

### 7.3 Примеры

Начальный поток:

- `CDM Context: провести абляции и сравнение порогов [CF#1]`

Повторный поток:

- `CDM Context: провести абляции и сравнение порогов [CF#2] / recalibrate tau_soft after failed eval`

Текущая очередь materialized потоков может сохранять свои существующие
читаемые titles до тех пор, пока поток не войдет в активное управление
`ChangeFlow`. Это не относится к `DeltaRegistry`. Naming convention
применяется к новым issues и к issues, которые явно нормализуются в repeat chains.

---

## 8. Инварианты

1. Одна терминальная задача равна одному `ChangeFlow`.
2. Terminal exits происходят после `evaluate`, а не заменяют `evaluate`.
3. `repeat` создает новый issue; старый issue никогда не переоткрывается.
4. `delayed` также никогда не переоткрывает старый issue; поздний возврат
   создает новый issue.
5. `Result` является отдельной runtime-осью относительно terminal status.

---

## 9. Ссылки

- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](../MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)
- [MMCF-Operational-Work-Unit-Contract](../MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Delivery-Linear-Planning-Profile](./MMCF-Delivery-Linear-Planning-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-and-Applicability-Profile](../MMCF-Conflict-and-Applicability-Profile.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
