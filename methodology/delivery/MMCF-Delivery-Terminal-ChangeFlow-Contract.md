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

---

## 3. Входные данные и шаблон body

### 3.1 Обязательные метаданные issue на intake

При создании терминальной задачи задайте как минимум:

1. title согласно правилам именования;
2. `Parent Epic`;
3. `LC Phase Snapshot`;
4. `Artifact Type`;
5. `Claim Mode`.

`Result` не является полем intake. Он выставляется только в `evaluate`.

Если workspace использует planning `v1.1`, дополнительно задайте:

1. `Flow Mode`
2. `Variativity Target`
3. `Decide Policy`
4. `MetaCF Risk`, когда уместно

### 3.2 Обязательные поля body

Каждый body терминальной задачи должен содержать как минимум:

1. `Parent Epic`
2. `Base task ref`
3. `Carrier entity`
4. `Base intent`
5. `Active context(s)`
6. `LC phase snapshot`
7. `Expected delta`
8. `Success criteria`
9. `Stop criteria`
10. `Evidence refs`

Рекомендуемый шаблон body задачи:

```md
## Canon
- Parent Epic:
- Base task ref:
- Carrier entity:
- Base intent:
- Active context(s):
- LC phase snapshot:

## Delivery
- Artifact type:
- Claim mode:
- Expected delta:
- Success criteria:
- Stop criteria:

## Planning
- Flow mode:
- Variativity target:
- Decide policy:
- MetaCF risk:
- max_decide_delay:
- forced_collapse_trigger:
- resource_cap:
- Planning rationale:

## PT / Gateways
- Default PT scenario: event
- Non-trivial transitions:
- Approval refs / owners:
- Claim refs / evidence refs:

## Evidence
- Main evidence refs:
- Related specs/docs:
- Constraints:
```

### 3.3 Руководство по `PT / Gateways`

Блок `PT / Gateways` используется для предварительного объявления
нетривиальных переходов.

Если все ожидаемые успешные переходы являются простыми `event`-переходами,
секция может оставаться минимальной:

- `Default PT scenario: event`
- `Non-trivial transitions: none`

Если ожидается approval или передача результата, несущего claims, укажите это явно, например:

- `forecast => decide: approve`
- `implement => evaluate: approve`
- `analyze => forecast: claim`

Это дает потоку явный план переходов до появления узких мест.

### 3.4 Руководство по `Planning`

Блок `Planning` описывает только task-side planning contour задачи.

Он может включать:

1. execution mode;
2. planned alternative width;
3. decision-collapse policy;
4. safety-hooks creative/delayed mode;
5. краткое обоснование planning допущений.

Он не должен включать:

1. executor capability values;
2. `Doubt` или `Subjectivity` bands как свойства задачи;
3. candidate ranking или suitability scores.

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
- PT trace refs:
- PT summary:
- Exit decision: done | repeat | final | delayed
- Zero class: AF_sem | AF_epi | AF_sto | mixed
- Repeat reason:
- Next CF issue:
- Return condition:
- Review date / trigger:
- Summary:
```

### 5.1 Руководство по `CF6`

1. `Failure class` объясняет, какой слой в первую очередь определил terminal
   outcome:
   - `phase-logic`
   - `applicability`
   - `gateway`
   - `mixed`
   - `none` для прямолинейного успешного завершения
2. `PT trace refs` указывают на явные gateway traces, когда:
   - требовался approval;
   - требовалась передача результата, несущего claims;
   - любой переход завершился неуспешно;
   - нетривиальный переход создал основное узкое место
3. `PT summary` является короткой итоговой заметкой, например:
   - `implicit event transitions only`
   - `implement => evaluate approve completed after owner review`
   - `forecast => decide approval stalled for 9d`
   - `implement => evaluate failed as gateway; handoff not accepted`
4. `Zero class` заполняется только когда `Result=0`.
5. `Repeat reason` и `Next CF issue` обязательны только для `repeat`.
6. `Return condition` и `Review date / trigger` обязательны только для
   `delayed`.

---

## 6. Правила выхода

Terminal exit выбирается после `CF6` из трех независимых вопросов:

1. какой `Result` наблюдался;
2. требуется ли немедленно следующий `ChangeFlow`;
3. если не немедленно, ожидается ли будущий возврат или нет.

Исходы `PhaseTransition` влияют на это решение, но не заменяют его.
Узкое место на gateway или задержка approval сами по себе не являются terminal exit.

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
2. `Applicable=true`, `Result=-1`, и корректирующий или компенсирующий поток должен
   начаться немедленно;
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

- `Applicable=false`
- `Result=0`
- планируемого возврата нет

Используйте `final`, когда текущий поток закрывает линию без немедленного или
ожидаемого будущего продолжения.

Типовые валидные случаи:

1. canonical или управленческое решение установило реальную неприменимость;
2. approval или review по claims завершились стабильным решением "не продолжать
   эту линию";
3. текущая формулировка отклонена без планируемой reformulation.

PT-aware note:

1. failed approval или claim transition не означает автоматически `final`;
2. `final` корректен только тогда, когда исход transition устанавливает
   стабильную неприменимость, а не просто задержку.

Обязательно:

- причина финальной остановки

### 6.4 `delayed`

Значение по умолчанию:

- `Applicable=false`
- `Result=0`
- возврат ожидается позже

Используйте `delayed`, когда текущий поток неприменим сейчас, но известен или
ожидается будущее условие корректного возврата.

Типовые валидные случаи:

1. отсутствует заморозка вышестоящей спецификации;
2. ожидается управленческое решение;
3. не хватает evidence или завершения protocol;
4. недоступна зависимость, окружение или окно ресурсов.

PT-aware note:

1. само по себе длительное время approval недостаточно для `delayed`;
2. `delayed` корректен только тогда, когда поток закрывается потому, что
   следующий валидный ход зависит от более позднего условия, а не от
   немедленной корректирующей работы.

Обязательно:

- `Return condition`
- `Review date / trigger`
- владелец возврата

### 6.5 Матрица решений

Используйте следующее отображение по умолчанию:

1. `Result=+1` и немедленный следующий `CF` не нужен -> `done`
2. `Result=+1` и нужен немедленный следующий `CF` -> `repeat`
3. `Result=-1` и нужен немедленный corrective/compensating `CF` -> `repeat`
4. `Result=0` и есть немедленно reformulated `CF` -> `repeat`
5. `Result=0` и немедленного следующего `CF` нет, но ожидается будущий возврат ->
   `delayed`
6. `Result=0` и немедленного следующего `CF` нет, и возврат не планируется -> `final`

Выделенного terminal stop status для `Result=-1` без немедленного следующего
потока в `v1` не существует.

Поэтому такие случаи должны оставаться в `evaluate`, пока не произойдет одно
из следующих событий:

1. создан корректирующий `repeat` issue;
2. policy или governance переоформят кейс в `Applicable=false`, `Result=0`, а
   затем закроют его как `final` или `delayed`.

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

Текущий backlog может сохранять свои существующие читаемые titles до тех пор,
пока задача не войдет в активное управление `ChangeFlow`. Naming convention
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

- [MMCF-Operational-Work-Unit-Contract](../MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Delivery-Linear-Planning-Profile](./MMCF-Delivery-Linear-Planning-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-and-Applicability-Profile](../MMCF-Conflict-and-Applicability-Profile.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
