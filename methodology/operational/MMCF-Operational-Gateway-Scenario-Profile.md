---
title: "MMCF: Operational Gateway Scenario Profile"
date: 2026-03-12
tags: [MMCF, gateway, PhaseTransition, operational, profile]
status: profile-draft
---

# MMCF: Operational Gateway Scenario Profile

## 1. Scope

Этот документ задает operational vocabulary для типовых сценариев `CF-PT`,
используемых в MMCF поверх канонического `PhaseTransition-CF`.

Профиль не заменяет каноническую CDM-классификацию `conditional /
unconditional`. Он вводит только удобные operational scenarios для наблюдаемости
и управления.

Разделение `phase state` и `transition state`, на которое этот профиль
опирается, вынесено отдельно в:

- [MMCF-Phase-and-Transition-State-Separation-Profile](./MMCF-Phase-and-Transition-State-Separation-Profile.md)

Общий принцип примата канона над ограничениями внешней системы вынесен
отдельно в:

- [MMCF-Canonical-to-External-System-Mapping-Profile](./MMCF-Canonical-to-External-System-Mapping-Profile.md)

Reusable шаблонный слой для типовых `PT`, не меняющий канон, вынесен отдельно в:

- [MMCF-PT-Template-Catalog-Profile](./MMCF-PT-Template-Catalog-Profile.md)

---

## 2. CDM foundation

Профиль опирается на следующие канонические положения CDM:

1. `CF-PT` является автономным процессом передачи и согласования между фазами.
2. Переход имеет собственный `result in {true,false}` и собственное время.
3. terminal `pt_result=false` прерывает текущий `ChangeFlow` и ведет к `CF6`.
4. Failure transition не является автоматическим доказательством ошибки обеих
   соседних фаз.

---

## 3. Canon Boundary

Каноническая CDM-классификация остается такой:

1. `unconditional`
2. `conditional`

Operational scenarios этого профиля используются как дополнительный слой
интерпретации и журналирования.

---

## 4. Operational Scenarios

### 4.1 `event`

Простой переход, где отдельный процесс согласования или claim-acceptance не
требуется.

Обычно:

1. результат предыдущей фазы передается напрямую;
2. следующая фаза может стартовать сразу после принятия handoff;
3. сценарий обычно соответствует `unconditional`.

### 4.2 `approve`

Переход требует явного подтверждения, финального согласования или owner verdict.

Обычно:

1. review gate;
2. owner approval;
3. release/policy/security gate;
4. сценарий обычно соответствует `conditional`.

### 4.3 `claim`

Переход требует валидной передачи результата, несущего claims, protocol или
evidence package.

Обычно:

1. следующая фаза не может стартовать без claim/evidence acceptance;
2. сценарий обычно соответствует `conditional`.

### 4.4 `approve+claim`

Переход требует одновременно:

1. explicit approval;
2. claim/evidence acceptance.

Сценарий обычно соответствует `conditional`.

---

## 5. Minimal Operational Trace Additions

Если operational system использует этот профиль, рекомендуется фиксировать:

1. `pt_class = unconditional | conditional`
2. `pt_form = inline | process-based`
3. `pt_scenario = event | approve | claim | approve+claim`
4. `approval_ref`, когда approval materially required
5. `claim_ref` или `evidence_ref`, когда передается claim-bearing result
6. `failure_reason`, когда `result=false`
7. `pt_failure_policy`, когда используется не `fail_fast`
8. `attempt_no`, когда переход уже ретраился
9. `policy_state = retrying | exhausted | completed`

Нормативно:

1. `pt_class`, `pt_form` и `pt_scenario` являются разными осями описания перехода;
2. operational form не заменяет канонический `pt_class`;
3. operational scenario не заменяет ни канонический `pt_class`, ни `pt_form`;
4. scenario должен объяснять реальный bottleneck, а не просто дублировать
   название следующей фазы.

Практические defaults допустимы, но не являются тождествами:

1. `event` обычно соответствует `unconditional + inline`;
2. `approve`, `claim`, `approve+claim` обычно соответствуют
   `conditional + process-based`;
3. возможны и другие комбинации, например:
   - `conditional + inline` для синхронного автоматического gate;
   - `unconditional + process-based` для обязательного handoff через отдельный
     transport contour.

---

## 6. Retry And Recovery Policy

MMCF допускает operational retry/recovery policy внутри одного и того же
`CF-PT`, пока не зафиксирован terminal `pt_result=false`.

Для этого профиля различаются:

1. `pt_attempt_failure`
   Неуспешная отдельная попытка перехода внутри продолжающегося `In-Transition`.
2. `terminal_pt_failure`
   Финальный неуспех перехода после `fail_fast` или исчерпания retry policy.

Нормативно:

1. `pt_attempt_failure` сам по себе не обязан немедленно прерывать текущий
   `ChangeFlow`;
2. только `terminal_pt_failure` приводит к фиксации `pt_result=false`;
3. только после terminal `pt_result=false` текущий `ChangeFlow` идет в `CF6`
   по канону CDM.

Допустимые policy values:

1. `fail_fast`
2. `retry_n(n)`
3. `retry_until_deadline(deadline)`
4. `retry_until_condition(condition_ref)`

Для `retry_until_condition(condition_ref)` условие должно быть:

1. внешне наблюдаемым;
2. проверяемым по trace/evidence;
3. связанным с явным stop rule или deadline, если оно не гарантировано наступит
   само.

Практически допустим и policy-смысл `retry_until_success`, но только если он
сведен к одной из двух явных ограниченных форм:

1. `retry_until_deadline(...)`
2. `retry_until_condition(...)`

Неограниченное бесконечное `retry_until_success` без deadline, condition или
внешнего stop rule не рекомендуется.

---

## 7. Failure And Responsibility Semantics

При использовании operational scenarios действуют те же базовые правила MMCF/CDM:

1. phase responsibility и gateway responsibility остаются раздельными;
2. длинная approval queue является bottleneck уровня transition, а не
   автоматически failure предыдущей или следующей фазы;
3. failed `claim` acceptance также является gateway-level failure, пока не
   доказано обратное;
4. terminal exit decision work unit не подменяет собой gateway classification.

---

## 8. Anti-Patterns

Запрещено или нежелательно:

1. подменять `conditional/unconditional` одними operational labels;
2. кодировать любой review как failure соседней фазы;
3. считать любой claim-bearing handoff обязательным `approve+claim` без
   реальной double-gate;
4. использовать scenario label без явной trace, когда переход materially
   определил outcome потока.
5. фиксировать `pt_result=false`, а затем продолжать тот же `ChangeFlow`,
   как будто terminal failure еще не произошел.
6. использовать неограниченное `retry_until_success` без deadline, condition
   или внешнего stop rule.

---

## 9. Normative References

- [MMCF-Canonical-to-External-System-Mapping-Profile](./MMCF-Canonical-to-External-System-Mapping-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Phase-and-Transition-State-Separation-Profile](./MMCF-Phase-and-Transition-State-Separation-Profile.md)
- [MMCF-PT-Template-Catalog-Profile](./MMCF-PT-Template-Catalog-Profile.md)
- [CDM PhaseTransition-CF](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Claim Maturity Profile](../../../fcdm-core/theory/cdm/Specifications/Claim-Maturity-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](../delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
