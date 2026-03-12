---
title: "MMCF Delivery: профиль Linear"
date: 2026-03-12
tags: [MMCF, delivery, Linear, profile, ChangeFlow, LifeCycle]
status: profile-draft
---

# MMCF Delivery: профиль Linear

## 1. Область

Этот документ определяет, как MMCF/CDM-сущности delivery отображаются в
Linear для разработки ПО и работы с документацией.

Профиль привязан к инструменту и не переопределяет `canon` MMCF или CDM.

Логика декомпозиции и границ для этого профиля отдельно суммирована в:

- [MMCF-Delivery-AppliedRules-Integration-Profile](./MMCF-Delivery-AppliedRules-Integration-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Delivery-Linear-Planning-Profile](./MMCF-Delivery-Linear-Planning-Profile.md)
- [MMCF-Delivery-Linear-Versioning-Profile](./MMCF-Delivery-Linear-Versioning-Profile.md)

Общие MMCF-смыслы, на которые этот tool-profile опирается напрямую:

- [MMCF-Operational-Work-Unit-Contract](../MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Operational-Gateway-Scenario-Profile](../MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Claim-Governance-Applicability-Profile](../MMCF-Claim-Governance-Applicability-Profile.md)
- [MMCF-Planning-Assignment-Capability-Boundary](../MMCF-Planning-Assignment-Capability-Boundary.md)

---

## 2. Каноническое отображение

| Объект Linear | Отображение MMCF/CDM | Правило |
|---|---|---|
| `Project` | родительский экземпляр `LifeCycle` | Использовать только там, где существует верхнеуровневый системный контур. |
| `Epic` | дочерний экземпляр `LifeCycle` | Использовать только если кандидат проходит `RLC/CC`. |
| терминальная задача | один `ChangeFlow` | Один issue равен полному `CF1..CF6`. |
| комментарий issue в `evaluate` | трасса `CF6` | Обязателен для фиксации результата. |
| versioning block | snapshot carrier entity | Не подменять им отдельную версию work item. |
| labels/custom fields | оси контекста и управления | Не моделировать их как иерархические уровни. |

---

## 3. Правила декомпозиции

### 3.1 `Project -> Epic`

`Epic` валиден только тогда, когда он является отдельным дочерним lifecycle
относительно проектного контура:

1. у него есть граница;
2. у него есть lifecycle-подобный режим существования;
3. он может нести несколько содержательных `ChangeFlow`;
4. у него есть собственный критерий завершения/трансформации.

Если эти условия не выполнены, работа остается непосредственно под `Project`
в виде терминальных задач.

### 3.2 `Epic -> терминальная задача`

Терминальные задачи никогда не трактуются в этом профиле как lifecycle-сущности.
Они являются `ProcessOf(Epic)` и должны представлять ровно один `ChangeFlow`.

### 3.3 Граница контекста

`Context` в этом профиле не представляется как `Epic`.
Внешние влияния остаются в полях, метках и комментариях с evidence.

---

## 4. Модель статусов для терминальных задач

Статусы представляют фазовый слой рабочего процесса.

Шлюзы и наблюдаемость для `PhaseTransition` определены отдельно в:

- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)

Versioning для `Epic` и terminal issue определен отдельно в:

- [MMCF-Delivery-Linear-Versioning-Profile](./MMCF-Delivery-Linear-Versioning-Profile.md)

### 4.1 Статусы Linear

Рекомендуемые статусы терминальной задачи:

- `Backlog`
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
- `duplicate`

### 4.2 Отображение на type в Linear

- `Backlog` -> `backlog`
- `Todo` -> `unstarted`
- `collect/analyze/forecast/decide/implement/evaluate` -> `started`
- `done/repeat` -> `completed`
- `final/delayed/duplicate` -> `canceled`

### 4.3 Правила переходов

- `Todo -> collect -> analyze -> forecast -> decide`
- `decide -> implement`, если `Applicable=true`
- `decide -> evaluate`, если `Applicable=false`
- `implement -> evaluate`
- `evaluate -> done | repeat | final | delayed`

Фазу `evaluate` пропускать нельзя.

Если `PT` неуспешен, но retry policy еще не исчерпана:

1. issue не переходит автоматически в `evaluate`;
2. issue остается в статусе исходной фазы;
3. состояние перехода отражается gateway trace и, при необходимости,
   bottleneck labels.

---

## 5. Семантика terminal-статусов

- `done` означает, что текущий `ChangeFlow` завершен и следующий `CF` не нужен.
- `repeat` означает, что текущий `ChangeFlow` завершен и следующий `CF` должен
  быть создан немедленно.
- `final` означает `Applicable=false`, `Result=0` и отсутствие ожидания
  дальнейшего возобновления линии.
- `delayed` означает `Applicable=false`, `Result=0` и ожидание возврата позже,
  когда изменится контекст или правила.

`repeat` и `delayed` нельзя путать:

- `repeat` -> создать следующий issue сейчас;
- `delayed` -> не создавать следующий issue сейчас, а сохранить условие возврата.

Матрица terminal-решений по умолчанию:

1. `Result=+1` и немедленный следующий `CF` не нужен -> `done`
2. `Result=+1` и нужен немедленный следующий `CF` -> `repeat`
3. `Result=-1` и нужен немедленный corrective/compensating `CF` -> `repeat`
4. `Result=0` и есть немедленно переформулированный `CF` -> `repeat`
5. `Result=0` и немедленного следующего `CF` нет, но ожидается будущий возврат ->
   `delayed`
6. `Result=0` и немедленного следующего `CF` нет, и возврат не планируется -> `final`

Отдельного terminal stop status для `Result=-1` без немедленного следующего
потока в `v1` нет. Такие случаи должны оставаться в `evaluate`, пока не случится
одно из двух:

1. создан corrective `repeat` issue;
2. policy/governance явно переоформляет кейс в `final` или `delayed` через
   решение о неприменимости.

---

## 6. `Result` и evidence

`Result` не кодируется статусом issue.

Минимально обязательные поля трассы исполнения:

1. `Applicable`
2. `Result`
3. `failure_class`
4. `active_context`
5. `lc_phase_snapshot`
6. `evidence_refs`
7. `pt_trace_refs`, если происходили нетривиальные или failed transitions
8. `exit_decision`
9. `zero_class`, когда `Result=0`
10. `next_cf_issue` для `repeat`
11. `return_condition` для `delayed`

Все они должны присутствовать в обязательном комментарии `evaluate`.

### 6.1 Custom fields v1

Для первой практической схемы Linear в custom fields выносится только
стабильное и компактное подмножество:

1. `LC Phase Snapshot`
   Допустимые значения: `F1 | F2 | F3 | F4 | F5 | F6`
2. `Result`
   Допустимые значения: `+1 | 0 | -1`
3. `Artifact Type`
   Допустимые значения: `software | documentation | scientific-documentation`
4. `Claim Mode`
   Допустимые значения: `claim-none | claim-optional | claim-required`

Этих четырех полей достаточно для первоначальной фильтрации и отчетности без
жесткого вталкивания высокоэнтропийных данных исполнения в фиксированные слоты полей.

Это минимально обязательный набор `v1`.
Аддитивное расширение planning `v1.1` для `Flow Mode`, `Variativity Target`,
`Decide Policy` и task-side planning block определено отдельно в
[MMCF-Delivery-Linear-Planning-Profile](./MMCF-Delivery-Linear-Planning-Profile.md).

Executor matching и capability registry намеренно остаются вне этого
tool-profile и вынесены в корневой слой `methodology/`.

Заметка по жизненному циклу полей:

1. выставляйте `LC Phase Snapshot`, `Artifact Type` и `Claim Mode` при
   создании issue;
2. выставляйте `Result` только в `evaluate`, когда исход потока уже известен.

### 6.2 Данные, которые остаются вне custom fields в v1

Следующие элементы намеренно не выносятся в custom fields в первой версии схемы:

1. `CF phase`
   Уже кодируется статусом issue.
2. `exit_decision`
   Уже кодируется terminal status.
3. `previous_cf_issue` и `next_cf_issue`
   Лучше представлены связями между issues.
4. `return_condition`
   Лучше хранить в комментарии `evaluate`, потому что это обычно текст.
5. `active_context`
   Лучше хранить в body issue и комментарии `evaluate`, пока таксономия
   активного контекста не стала стабильнее.
6. version snapshots (`ObservedVersion`, `InputVersion`, `PostCFVersion`, `CFIndex`)
   Лучше хранить в body issue и `CF6`, а не в top-level custom fields.

### 6.3 Отложенные кандидаты

Следующие поля можно добавить позже, если практика покажет устойчивое
использование:

1. `Zero Class`
2. `Canon Compatibility`
3. `Review Trigger`

---

## 7. Labels v1

Labels в этом профиле являются сквозными сигналами уровня issue.

Они не должны заменять:

1. phase statuses;
2. terminal exits;
3. явную трассу `PhaseTransition`.

### 7.1 Существующие baseline labels

Следующие уже существующие labels остаются валидной базовой классификацией issue:

1. `Bug`
2. `Feature`
3. `Improvement`
4. `Chore`
5. `Codex`
6. `Triage`
7. `Blocked`

### 7.2 Дополнительные labels для v1

Рекомендуемые дополнительные labels:

1. `AwaitingApproval`
   Активное узкое место находится в переходе approval/финального согласования.
2. `AwaitingClaim`
   Активное узкое место находится в claim/evidence/canon acceptance.
3. `GatewayFailure`
   Последний переход завершился неуспешно и требует явного восстановления.
4. `NeedsEvidence`
   Issue не может валидно пройти `evaluate` или перехода с результатом, несущим claims, без
   дополнительных evidence.
5. `Governance`
   Issue требует участия policy, canon или governance сверх обычного
   исполнения фазы.
6. `ClaimUpdate`
   Issue меняет контент, несущий claims, или имеет явное влияние на claim-status.

### 7.3 Правила использования

1. `Blocked` зарезервирован для внешних blockers, не относящихся к `PT`.
2. `AwaitingApproval`, `AwaitingClaim` и `GatewayFailure` являются
   вспомогательными сигналами видимости уровня issue для активного узкого места перехода.
3. `AwaitingApproval`, `AwaitingClaim` и `GatewayFailure` не заменяют явный
   gateway trace, определенный в профиле `PhaseTransition`.
4. Обычно одновременно должен использоваться только один активный label
   узкого места: `Blocked`, `AwaitingApproval`, `AwaitingClaim` или `GatewayFailure`.
5. `NeedsEvidence`, `Governance` и `ClaimUpdate` могут сосуществовать с
   меткой узкого места.

---

## 8. Заметка об именовании

Заголовки терминальных задач должны следовать naming convention, определенной в:

- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)

Заголовок issue идентифицирует стабильную цель потока и индекс цепочки повторов.
Он не должен кодировать phase status, terminal exit или активное узкое место
`PhaseTransition`.

---

## 9. Инварианты

1. Одна терминальная задача равна одному `ChangeFlow`.
2. `evaluate` обязателен перед любым terminal exit.
3. `done` по умолчанию допустим только при `Result=+1`.
4. `repeat` допустим только если следующий sibling issue уже существует.
5. `final` и `delayed` по умолчанию допустимы только при `Result=0`.
6. `Context`, `C_obs`, `C_coord`, `C_meta` не являются уровнями иерархии.

---

## 10. Заметка о метриках

Поскольку `final` и `delayed` отображаются в type `canceled` в Linear,
нативные completed-метрики в Linear будут считать главным образом применимые
завершенные потоки.

Это приемлемо в текущем профиле, потому что:

1. applicable-completion и inapplicable-stop намеренно разведены;
2. `Result=0` остается видимым в трассе исполнения;
3. будущая аналитика может агрегировать все terminal exits отдельно от
   нативных completed-counters Linear.

---

## 11. Ссылки

- [MMCF-Canonical](../MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](../MMCF-Minimal-Working-Model.md)
- [MMCF-Operational-Work-Unit-Contract](../MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Operational-Gateway-Scenario-Profile](../MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Claim-Governance-Applicability-Profile](../MMCF-Claim-Governance-Applicability-Profile.md)
- [MMCF-Planning-Assignment-Capability-Boundary](../MMCF-Planning-Assignment-Capability-Boundary.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-and-Applicability-Profile](../MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Delivery-AppliedRules-Integration-Profile](./MMCF-Delivery-AppliedRules-Integration-Profile.md)
- [CDM LifeCycle-6](../../../fcdm-core/theory/cdm/Specifications/LifeCycle-6_v2.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [CDM RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md)
- [CDM SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md)
- [CDM Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md)
