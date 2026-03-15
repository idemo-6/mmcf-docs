---
title: "MMCF: layered memory guide for corpus review"
date: 2026-03-15
tags: [MMCF, Codex, corpus-review, layered-memory, guide]
status: working-draft
---

# MMCF: layered memory guide for corpus review

## 1. Зачем это нужно

Этот guide нужен для практического анализа больших корпусов связанных
текстовых файлов, когда:

- файлов много;
- связи между ними важнее локальных summary;
- один и тот же корпус может читаться под разными вопросами;
- нельзя полагаться только на оперативную память модели.

Главная задача guide:

1. не заставлять LLM каждый раз заново собирать весь глобальный контекст;
2. развести долговременную память о корпусе, рамку исследования и рабочие
   наблюдения текущего прохода;
3. сделать анализ трассируемым и пригодным для повторных исследований.

Этот guide не про `Context` в каноническом смысле `MMCF/CDM/CTxL`.

Он не про:

1. `C_active`;
2. `C_candidates`;
3. `Applicable`;
4. `context-conflict`.

Он про рабочую memory-организацию исследования корпуса в `Codex` и
repo-first knowledge workflow.

## 2. Базовая модель

Используйте три разных слоя:

1. `corpus-context`
2. `research-context`
3. `scratchpad`

Короткая формула:

`corpus-context -> research-context -> scratchpad`

Где:

1. `corpus-context` хранит долговременные сущности и отношения корпуса;
2. `research-context` хранит рамку конкретного исследования;
3. `scratchpad` хранит наблюдения, pending, conflicts и reconciliation.

Обратное движение идет только через promotion-pass.

## 3. Что хранит каждый слой

### `corpus-context`

Это долговременная память корпуса.

Здесь хранить:

1. устойчивые сущности;
2. устойчивые отношения;
3. канонические термины и aliases;
4. повторяющиеся tension/conflict линии;
5. карту центральных и периферийных кластеров.

Сюда нельзя писать напрямую после чтения одного файла.

### `research-context`

Это рамка конкретного исследования.

Здесь хранить:

1. главный вопрос;
2. secondary questions;
3. scope;
4. out-of-scope;
5. imported priors из `corpus-context`;
6. working hypotheses;
7. relevance criteria;
8. expected output;
9. bias risks;
10. change log рамки.

### `scratchpad`

Это оперативная память текущего прохода.

Здесь хранить:

1. observations;
2. candidate links;
3. pending fragments;
4. conflicts;
5. reconciliation notes;
6. proposed updates to `research-context`;
7. proposed updates to `corpus-context`.

Именно сюда писать в первую очередь во время чтения.

## 4. Рекомендуемая раскладка

Базовый layout:

```text
_codex_corpus_review/
  corpus-context.md
  studies/
    <study-id>/
      research-context.md
      scratchpad.md
      file-notes/
      final-report.md
```

Если исследование одноразовое, допустим упрощенный вариант:

```text
_codex_corpus_review/
  corpus-context.md
  research-context.md
  scratchpad.md
  file-notes/
  final-report.md
```

Но как только по одному корпусу появляются независимые вопросы, лучше
переходить на `studies/<study-id>/`.

## 5. Порядок чтения

Перед чтением файлов корпуса читать в таком порядке:

1. `corpus-context`
2. `research-context`
3. `scratchpad`
4. затем файлы корпуса

Это удерживает:

1. долговременную память корпуса;
2. текущую рамку исследования;
3. уже найденные наблюдения текущего прохода.

## 6. Порядок записи

Во время работы:

1. сначала `file-note`;
2. потом `scratchpad`;
3. затем при необходимости `research-context`;
4. и только в отдельном promotion-pass — `corpus-context`.

Прямой write в `corpus-context` из первичного file-pass запрещен.

## 7. Promotion rules

### `scratchpad -> research-context`

Делать только если изменилась рамка исследования:

1. уточнился вопрос;
2. сместился scope;
3. поменялась рабочая гипотеза;
4. понадобился новый sub-question;
5. часть корпуса стала явно out-of-scope.

### `scratchpad -> corpus-context`

Делать только если наблюдение:

1. устойчиво на уровне корпуса;
2. не question-specific;
3. подтверждено достаточным evidence;
4. имеет смысл для будущих исследований по тому же корпусу.

## 8. Anti-patterns

Не делать так:

1. один `scratchpad` как смесь corpus-memory, research-frame и рабочих заметок;
2. запись в `corpus-context` после первого же файла;
3. превращение текущей гипотезы в corpus-level истину;
4. повторное использование одного `research-context` для разных несводимых вопросов;
5. тихое изменение рамки исследования без `change log`.

## 9. Минимальные разделы файлов

### `corpus-context.md`

1. `Corpus Identity`
2. `Stable Entities`
3. `Stable Relations`
4. `Canonical Terms and Aliases`
5. `Stable Tensions and Conflicts`
6. `Central Clusters`
7. `Peripheral Patterns`
8. `Promotion Log`

### `research-context.md`

1. `Research Identity`
2. `Main Question`
3. `Secondary Questions`
4. `Scope`
5. `Out of Scope`
6. `Imported Corpus Priors`
7. `Working Hypotheses`
8. `Relevance Criteria`
9. `Expected Output`
10. `Bias Risks`
11. `Change Log`

### `scratchpad.md`

1. `Run Scope`
2. `Current Interpretation`
3. `Observations`
4. `Candidate Links`
5. `Pending`
6. `Conflicts`
7. `Proposed Updates to Research Context`
8. `Proposed Updates to Corpus Context`
9. `Reconciliation Decisions`
10. `Next Reads`
11. `Change Log`

## 10. Практический цикл

Минимальный цикл:

1. прочитать `corpus-context`;
2. зафиксировать или уточнить `research-context`;
3. создать `scratchpad` и `file-notes`;
4. пройти batch файлов;
5. выполнить reconciliation;
6. обновить `research-context`, если реально изменилась рамка;
7. в отдельном promotion-pass обновить `corpus-context`, если появились
   durable findings;
8. собрать `final-report` из всех трех слоев, а не только из памяти модели.

## 11. Связь со skill

Для `Codex` skill анализа текстового корпуса это означает:

1. skill должен держать все три слоя;
2. skill не должен использовать старую плоскую модель одного
   `global-context`;
3. final answer должен показывать не только вывод, но и какие promotions были
   предложены или выполнены.
