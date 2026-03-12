---
title: "MMCF: Operational Work Unit Contract"
date: 2026-03-12
tags: [MMCF, operational, work-unit, ChangeFlow, closure]
status: profile-draft
---

# MMCF: Operational Work Unit Contract

## 1. Scope

Этот документ задает общий MMCF-контракт для операционной единицы работы,
которая управляется поверх канонического `ChangeFlow`.

Документ не переопределяет `ChangeFlow-6`, `CF-LC Evaluate` или правила
классификации `Subsystem / ProcessOf`. Он задает только общий operational-layer
для систем управления задачами, delivery и смежных контуров исполнения.

---

## 2. CDM foundation

Контракт опирается на следующие канонические положения CDM:

1. `ChangeFlow` является минимально полным атомарным циклом изменения.
2. `CF5` допустим только после валидного `Applicable(Intent, C_active, LC_phase)=true`.
3. `CF6` обязателен как точка фиксации `Result` и влияния `CF -> LC`.
4. Процессный рабочий объект, не являющийся отдельной системой, трактуется как
   `ProcessOf(parent)`.

Следовательно, MMCF не вводит вторую альтернативную атомарную единицу
изменения поверх CDM, а фиксирует operational-contract для ее управления.

### 2.1 Pre-flow boundary

До materialization terminal work unit линия изменения может существовать как:

1. `Delta`;
2. `DeltaRecord`;
3. запись в `DeltaRegistry`;
4. triage/promotion decision.

Нормативно:

1. `DeltaRecord` не является `ChangeFlow`;
2. `DeltaRegistry` не является очередью operational work units;
3. operational work unit начинается только после `PromotionDecision=promote`;
4. `Accepted` является verdict triage-слоя, а не заменой operational state
   work unit;
5. pre-start state materialized work unit должен называться `Queued`, а не
   `Backlog`.

---

## 3. Definition

`Operational work unit` в MMCF — это ограниченный, управляемый и трассируемый
процессный объект, представляющий один полный `ChangeFlow` над конкретным
носителем изменения внутри родительского контура.

Нормативные правила:

1. одна operational work unit равна одному `ChangeFlow`;
2. work unit не является отдельным узлом `LifeCycle`, если она классифицирована
   как `ProcessOf(parent)`;
3. доменные алиасы фаз допустимы, но каноническая трасса остается `CF1..CF6`;
4. делегированная проекция `CF5-only` не считается самостоятельной полной work
   unit без внешнего контура, который берет на себя `CF1..CF4` и `CF6`.

Operational pre-start states:

1. `Queued`
2. `Todo`

Граница:

- `Queued` = work unit уже materialized после promotion, но еще не взят в
  активное фазовое исполнение;
- `Todo` = work unit уже допущен к исполнению и готов войти в `CF1`.

### 3.1 Minimal role and authority trace

Для каждого materialized work unit должен быть различим минимальный operational
trace ownership и authority.

Минимально:

1. `flow_id`
2. `delta_ref`
3. `intent_ref`
4. `cfowner_ref`
5. `assignee_ref` или `active_executor_ref`
6. `current_cf_phase`
7. `current_lc_phase`
8. `risk_class`
9. `role_trace_ref` или inline role block
10. `authority_map_ref`

Если применяется `Level 2` role profile, в role trace должны быть различимы:

1. `CFOwner`
2. текущий `CFPhaseOwner`
3. `TransitionOwner`, если активен нетривиальный `PT`
4. `DecisionAuthority`
5. `CommitAuthority`, если поток может входить в `CF5`
6. `EvaluationAuthority`
7. `GatewayApprovalAuthority`, если переход approve-bearing
8. `ClaimAuthority`, если поток claim-bearing

Нормативно:

1. `assignee_ref` не является каноническим синонимом `cfowner_ref`;
2. отсутствие различимого `cfowner_ref` или unreadable authority map делает
   work unit operationally degraded;
3. unresolved `authority-conflict` должен быть связан с work unit trace, а не
   оставаться вне carrier потока.

### 3.2 Minimal machine-readable skeleton

Ниже приведен минимальный skeleton, достаточный для tool/runtime mapping без
преждевременного перехода к полной runtime schema.

```yaml
flow_id: CF-2026-001
delta_ref: delta://registry/2026-03-12/001
intent_ref: intent://flow/CF-2026-001
risk_class: normal
state:
  current_lc_phase: F3
  current_cf_phase: decide
ownership:
  cfowner_ref: agent://owner/yasha
  assignee_ref: agent://executor/yasha
  cf_phase_owner_ref: agent://phase/decider
  transition_owner_ref: null
authority:
  decision_authority_ref: agent://authority/decider
  commit_authority_ref: null
  evaluation_authority_ref: agent://authority/reviewer
  gateway_approval_authority_ref: null
  claim_authority_ref: null
trace:
  role_trace_ref: trace://flow/CF-2026-001/roles
  authority_map_ref: trace://flow/CF-2026-001/authority
  last_handoff_ref: null
  last_override_ref: null
conflicts:
  active_authority_conflict: false
  authority_conflict_ref: null
```

Минимальные machine-readable event classes:

1. `role_handoff_event`
2. `authority_override_event`
3. `authority_conflict_event`

Минимальные поля `role_handoff_event`:

1. `flow_id`
2. `from_role`
3. `from_agent`
4. `to_role`
5. `to_agent`
6. `effective_at`
7. `reason`
8. `phase_snapshot`

Минимальные поля `authority_override_event`:

1. `flow_id`
2. `scope`
3. `overridden_authority`
4. `resolving_authority`
5. `precedence_basis`
6. `effective_at`
7. `reason`
8. `revalidation_required`

Минимальные поля `authority_conflict_event`:

1. `flow_id`
2. `scope`
3. `conflict_parties`
4. `conflict_class`
5. `detected_at`
6. `resolution_state`
7. `resolution_ref`

---

## 4. Carrier And Artifact Boundary

Work unit выполняется над `carrier entity`, а не над отдельным техническим
артефактом сопровождения.

Различайте:

1. `carrier entity`
   Сущность, состояние которой реально меняется данным потоком.
2. implementation artifacts
   Коммиты, pull requests, migrations, text patches, конфигурационные changes.
3. transition/governance artifacts
   Approvals, protocol handoffs, review verdicts, gate-comments.
4. evidence artifacts
   Test runs, validation reports, logs, measurements, canon references.

Нормативно:

1. артефакты реализации или проверки не тождественны самой work unit;
2. один поток может включать несколько supporting artifacts, если они служат
   одной и той же дельте над тем же carrier contour;
3. новый artifact сам по себе не означает новый `ChangeFlow`.

---

## 5. Closure Layer

Закрытие work unit является отдельным operational-layer поверх runtime result.

Нормативно:

1. work unit не может быть закрыта без `CF6`;
2. `Result in {+1,0,-1}` остается runtime-axis;
3. terminal exit остается отдельной operational-axis;
4. phase-trace и gateway-trace не заменяются terminal exit decision.

Для operational closure используется следующий набор значений:

1. `done`
2. `repeat`
3. `final`
4. `delayed`

---

## 6. Terminal Exit Semantics

### 6.1 `done`

Используется по умолчанию, когда:

1. текущий `ChangeFlow` завершен;
2. `Result=+1`;
3. немедленный следующий поток не требуется.

### 6.2 `repeat`

Используется, когда:

1. текущий `ChangeFlow` завершен;
2. следующий operational work unit должен начаться немедленно;
3. следующий поток сохраняет тот же contour, но несет новую или уточненную
   дельту.

Типовые валидные случаи:

1. `Result=+1`, но немедленно нужен следующий уточняющий проход;
2. `Result=-1`, и corrective/compensating flow должен начаться сразу;
3. `Result=0`, и линия немедленно reformulated в новый применимый поток.

### 6.2.1 Разрешение негативного исхода

Если `CF5` породил evidence неуспешного или деградационного эффекта, это еще не
означает, что `CF5` сам по себе фиксирует `Result=-1`.

Нормативно:

1. `CF5` порождает основание для verdict;
2. operational outcome по ветке `Result=-1` может разрешаться в
   `PT(CF5->CF6)`;
3. сам `Result=-1` фиксируется только в `CF6`;
4. вместе с `Result=-1` в `CF6` фиксируется и выбранный terminal exit:
   `repeat`, `final` или `delayed`.

Разрешение может происходить двумя способами:

1. через заранее заданную `negative_result_policy`;
2. через authority verdict на `PT(CF5->CF6)`, если этот переход является
   `approve` или `approve+claim`.

Допустимые operational значения `negative_result_policy`:

1. `auto_repeat`
2. `auto_final`
3. `auto_delayed`
4. `approve_exit`

### 6.3 `final`

Используется по умолчанию, когда:

1. текущая линия закрывается без немедленного и без ожидаемого будущего
   продолжения;
2. текущий `ChangeFlow` уже завершен и `CF6` зафиксирован.

Типовые валидные случаи:

1. `Result=0`, и каноническая или управленческая логика установила стабильную
   неприменимость;
2. `Result=-1`, и `negative_result_policy` либо authority verdict на
   `PT(CF5->CF6)` выбрали terminal stop;
3. линия сознательно закрывается как не подлежащая дальнейшему corrective
   продолжению.

### 6.4 `delayed`

Используется по умолчанию, когда:

1. немедленный следующий поток не нужен;
2. существует явное будущее условие корректного возврата.

Типовые валидные случаи:

1. `Result=0`, и линия сейчас неприменима, но ожидается будущий возврат;
2. `Result=-1`, и corrective continuation откладывается до наступления явного
   будущего условия;
3. policy или authority verdict прямо выбрали deferred return вместо
   немедленного `repeat`.

### 6.5 Default decision matrix

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
7. `Result=0` и немедленного следующего `CF` нет, но ожидается будущий возврат -> `delayed`
8. `Result=0` и немедленного следующего `CF` нет, и возврат не планируется -> `final`

Default rule for `v1`:

- `done` остается success-exit и не используется для `Result in {0,-1}`;
- `final` и `delayed` являются общими terminal canceled exits и не
  ограничиваются только `Result=0`;
- при `Result=0` operationally используются только `repeat`, `final` или
  `delayed`;
- при `Result=-1` ожидается, что `CF5` порождает evidence, `PT(CF5->CF6)`
  разрешает outcome по policy или authority verdict, а `CF6` фиксирует и
  `Result=-1`, и выбранный exit.

---

## 7. Repeat Chain And Return Rules

Для operational continuity действуют следующие правила:

1. `repeat` закрывает текущую work unit и создает новый следующий поток;
2. поздний возврат после `delayed` также создает новый поток, а не продолжает
   старый;
3. старый work unit не переоткрывается как каноническое продолжение;
4. следующий поток должен сохранять связь с тем же parent contour и базовой
   линией intent, если только governance явно не сменил contour.

---

## 8. Anti-Patterns

Запрещено или нежелательно:

1. считать commit, PR, review или comment самой operational work unit;
2. закрывать work unit без явного `CF6`;
3. смешивать terminal exit и runtime result в одну ось;
4. использовать `delayed` как скрытое имя для неопределенного `repeat later`;
5. переоткрывать старый work unit вместо запуска нового потока;
6. трактовать gateway delay или failed handoff как terminal exit сами по себе.
7. использовать `Backlog` как базовый методологический pre-start status
   terminal work unit.

---

## 9. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](./MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)
- [MMCF-Minimal-Working-Model](./MMCF-Minimal-Working-Model.md)
- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [CDM ChangeFlow-6](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [CDM RLC-CC Profile](../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM System Classification Profile](../../fcdm-core/theory/cdm/Specifications/System/System-Classification-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
