---
title: "MMCF: Conflict Taxonomy Canonical"
date: 2026-03-12
tags: [MMCF, conflict, Delta, taxonomy, canonical]
status: canonical-draft
---

# MMCF: Conflict Taxonomy Canonical

## 1. Scope

Документ задает канонически допустимые способы использования термина
`conflict` в MMCF.

Он не переопределяет `Delta`, `Intent`, `Applicable`, `ChangeFlow` или
`trigger-policy`. Его задача:

1. развести `Delta` и `conflict`;
2. зафиксировать минимальную taxonomy конфликтных классов;
3. определить non-conflict случаи `Delta`;
4. убрать двусмысленность между pre-flow несоответствием и собственно
   конфликтной несовместимостью.

---

## 2. Primary rule

В MMCF:

1. первичный pre-flow объект изменения — `Delta`;
2. `conflict` не является универсальным синонимом `Delta`;
3. `trigger` не является синонимом `conflict` и допустим только как
   policy-signal.

Следствие:

- не каждая `Delta` является `conflict`;
- не каждый `conflict` является основанием для немедленного `ChangeFlow`;
- `conflict` всегда вторичен относительно уже различимой `Delta`,
  несовместимости или divergence.

---

## 3. Conflict validity test

Термин `conflict` допустим только если можно указать хотя бы одну пару
операционно несовместимых элементов:

1. состояние vs референсное состояние;
2. контекст `Ci` vs контекст `Cj`;
3. ограничение `K1` vs ограничение `K2`;
4. authority/decision `A1` vs authority/decision `A2`;
5. resource demand `R1` vs resource demand `R2`.

Короткая форма:

`conflict(X, Y | C) := incompatible(X, Y | C) = true`

Если пара несовместимости не различима, используйте:

1. `Delta` без conflict-квалификации;
2. `opportunity`;
3. `monitor-only`;
4. `unknown/unqualified Delta`.

---

## 4. Conflict classes

### 4.1 `state-conflict`

`state-conflict` — это `Delta`, в которой актуальное состояние несовместимо с
референсным состоянием на одном carrier contour.

Форма:

`diff(S_actual, S_ref | C_active) != 0`

Типичный смысл:

- номер на сайте не совпадает с фактическим номером;
- документ противоречит актуальному канону;
- рабочая конфигурация расходится с допустимой.

### 4.2 `context-conflict`

`context-conflict` — несовместимость между контекстами, где одновременно не
удается получить единый исполнимый контур без координации, fork или block.

Это соответствует межконтекстной divergence/coordination проблеме и должно
читаться вместе с `CtxL` и `Context Coordination Protocol`.

### 4.3 `constraint-conflict`

`constraint-conflict` — несовместимость ограничений, при которой выполнение
одного требования нарушает другое.

Типичные случаи:

- срок конфликтует с quality gate;
- legal constraint конфликтует с business shortcut;
- local optimization конфликтует с global hard-stop.

### 4.4 `authority-conflict`

`authority-conflict` — несовместимость решений, approval или claims authority.

Типичные случаи:

- два authority-agent выдали несовместимые решения;
- owner и evaluator расходятся в допустимости хода;
- локальный override конфликтует с вышестоящей policy.

### 4.5 `resource-conflict`

`resource-conflict` — несовместимость требований к ограниченному ресурсу,
когда одновременное исполнение линий изменения невозможно без arbitration.

Типичные случаи:

- один слот исполнителя требуется разными потоками;
- compute/budget/time window недостаточны для совместного исполнения;
- критический путь блокируется конкурирующим allocation.

---

## 5. Non-conflict Delta classes

Следующие классы `Delta` не должны автоматически называться `conflict`:

### 5.1 `opportunity`

Появилась новая возможность, которая делает прежнюю конфигурацию
неоптимальной, но еще не содержит жесткой несовместимости.

### 5.2 `monitor-only`

`Delta` зафиксирована, но пока не требует разрешения через конфликтную логику
или promotion в `ChangeFlow`.

### 5.3 `unknown/unqualified Delta`

Факт несоответствия уже различим, но его класс еще не определен.

Нормативно:

- до квалификации лучше использовать `Delta`, а не `conflict`.

---

## 6. Layer mapping

Минимальное отображение по слоям:

1. pre-flow layer:
   - `Delta`
   - `state-conflict`
   - `opportunity`
   - `monitor-only`
2. context/coordination layer:
   - `context-conflict`
   - `constraint-conflict`
3. governance/delivery layer:
   - `authority-conflict`
   - `resource-conflict`

Следствие:

- не все conflict-классы должны жить в одном реестре;
- `DeltaRegistry` может хранить и conflict, и non-conflict классы;
- `C_coord` и `C_meta` работают прежде всего с context/constraint/authority
  конфликтами, а не с любой `Delta` без разбора.

---

## 7. Anti-patterns

Запрещено или нежелательно:

1. использовать `conflict` как универсальное имя любой `Delta`;
2. использовать `trigger` как общий синоним `conflict`;
3. называть `opportunity` конфликтом только потому, что она создает работу;
4. смешивать `state-conflict` с `context-conflict`;
5. считать любой `authority-conflict` доказательством `ApplicabilityFailure`;
6. создавать terminal `ChangeFlow` только потому, что слово `conflict`
   прозвучало в обсуждении.

---

## 8. Normative references

- [MMCF-Canonical](../core/MMCF-Canonical.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](../operational/MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)
- [MMCF-Term-Legacy-Mapping](../governance/MMCF-Term-Legacy-Mapping.md)
- [MMCF-Context-Canonical](./MMCF-Context-Canonical.md)
- [CDM Intent Spec](../../../fcdm-core/theory/cdm/Specifications/Intent-1.1.5.md)
- [CDM CtxL Canonical](../../../fcdm-core/theory/cdm/Specifications/CtxL/CtxL-Canonical.md)
- [CDM Context Coordination Protocol](../../../fcdm-core/theory/cdm/Specifications/Context/Context-Coordination-Protocol.md)
