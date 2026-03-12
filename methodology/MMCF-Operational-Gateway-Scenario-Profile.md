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

---

## 2. CDM foundation

Профиль опирается на следующие канонические положения CDM:

1. `CF-PT` является автономным процессом передачи и согласования между фазами.
2. Переход имеет собственный `result in {true,false}` и собственное время.
3. Сбой перехода прерывает текущий `ChangeFlow` и ведет к `CF6`.
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
2. `pt_scenario = event | approve | claim | approve+claim`
3. `approval_ref`, когда approval materially required
4. `claim_ref` или `evidence_ref`, когда передается claim-bearing result
5. `failure_reason`, когда `result=false`

Нормативно:

1. operational scenario не заменяет канонический `pt_class`;
2. scenario должен объяснять реальный bottleneck, а не просто дублировать
   название следующей фазы.

---

## 6. Failure And Responsibility Semantics

При использовании operational scenarios действуют те же базовые правила MMCF/CDM:

1. phase responsibility и gateway responsibility остаются раздельными;
2. длинная approval queue является bottleneck уровня transition, а не
   автоматически failure предыдущей или следующей фазы;
3. failed `claim` acceptance также является gateway-level failure, пока не
   доказано обратное;
4. terminal exit decision work unit не подменяет собой gateway classification.

---

## 7. Anti-Patterns

Запрещено или нежелательно:

1. подменять `conditional/unconditional` одними operational labels;
2. кодировать любой review как failure соседней фазы;
3. считать любой claim-bearing handoff обязательным `approve+claim` без
   реальной double-gate;
4. использовать scenario label без явной trace, когда переход materially
   определил outcome потока.

---

## 8. Normative References

- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Operational-Work-Unit-Contract](./MMCF-Operational-Work-Unit-Contract.md)
- [CDM PhaseTransition-CF](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
- [CDM ChangeFlow-6](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Claim Maturity Profile](../../fcdm-core/theory/cdm/Specifications/Claim-Maturity-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
