---
title: "MMCF: Context Example — Sales and CRM"
date: 2026-03-05
tags: [MMCF, Context, Sales, CRM, examples, Applicability]
status: profile-draft
---

# MMCF: Context Example — Sales and CRM

## 1. Назначение

Документ показывает прикладное применение контекстного канона MMCF на
примере домена \"Продажи + CRM\".

Цель:

1. показать, как выделять независимые контексты;
2. показать, когда допустим составной контекст;
3. показать, как это влияет на `Applicable` и `Result`.

---

## 2. Контексты примера

### 2.1 `C_sales_ops`

Операционный контекст продаж:

1. фокус: скорость и качество прохождения воронки;
2. ограничения: SLA ответа, скрипты, этапы сделки, ручные исключения;
3. KPI: conversion, cycle time, win rate.

### 2.2 `C_crm_automation`

Контекст автоматизации CRM:

1. фокус: корректность данных и автоматизаций;
2. ограничения: схема данных, интеграции, валидации, idempotency;
3. KPI: data quality, sync reliability, automation error rate.

---

## 3. Когда это два контекста

Вести как независимые контексты, если:

1. KPI успеха различаются;
2. hard-ограничения различаются;
3. владельцы решений различаются;
4. решение может быть применимо в одном контексте и неприменимо в другом.

Типичный кейс:

- Sales хочет добавить свободный этап воронки \"на лету\";
- CRM-контур не допускает это без миграции схемы и пересборки интеграций.

Итог:

- `Applicable=true` в `C_sales_ops`;
- `Applicable=false` в `C_crm_automation`.

---

## 4. Когда допустим составной контекст

`C_sales_crm_composite` допустим только если:

1. согласован единый KPI на задачу;
2. заранее сняты `constraint-conflict` между ограничениями;
3. есть единый owner решения;
4. возможно единое `Applicable`-решение.

Важно:

- составной контекст вводится для конкретного flow;
- он не отменяет базовые контексты как независимые для других задач.

---

## 5. Пример decision flow

Задача: \"Сократить время реакции на лидов в 2 раза\".

1. `CF1/CF2`:
   - фиксируются `C_candidates = {C_sales_ops, C_crm_automation}`;
   - фиксируются ограничения обоих контуров.
2. `CF3`:
   - формируются альтернативы: орг-изменение, CRM-автоматизация, гибрид.
3. `CF4`:
   - проверяется `Applicable` для выбранной траектории в активном контексте.

Вариант A:

- выбрана чисто организационная траектория без CRM-изменений;
- `Applicable=true` в `C_sales_ops`.

Вариант B:

- выбрана CRM-автоматизация без готовых schema/integration условий;
- `Applicable=false` в `C_crm_automation` -> `CF4 => CF6(inapplicable)` -> `Result=0`.

---

## 6. Интерпретация `Result=0` в примере

`Result=0` означает:

1. не \"провал команды\" как общий диагноз;
2. а неприменимость в текущем контексте и ограничениях.

Практическое действие после `Result=0`:

1. скорректировать контекстные ограничения;
2. сменить активный контекст;
3. запустить новый ChangeFlow с обновленными условиями.

---

## 7. Operational checklist для Sales/CRM

Перед стартом flow ответить:

1. Совпадает ли целевая функция Sales и CRM для этой задачи?
2. Совместимы ли hard-ограничения без дополнительного merge?
3. Назначен ли единый owner решения?
4. Можно ли корректно поставить единый `Applicable`-gate?
5. Что будет основанием для `Result=0`, если задача неприменима?

Если на 2+ вопроса ответ \"нет\":

- вести задачу как мультиконтекстную (без искусственного merge).

---

## 8. Anti-patterns (пример)

Запрещено:

1. считать Sales и CRM одним контекстом \"по орг-схеме\" без проверки
   ограничений;
2. объявлять `Result=0` как \"нейтрально сработало\";
3. выбирать `C_active` по политической силе стейкхолдера без явных правил.

---

## 9. Связь с каноном

Документ является прикладным profile-example и использует канон:

1. `CF1..CF6`;
2. `Applicable(Intent, C_active, LC_phase)`;
3. `Result=0 <=> ApplicabilityFailure`.

---

## 10. References

- [MMCF-Context-Canonical](./MMCF-Context-Canonical.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Conflict-Taxonomy-Canonical](./MMCF-Conflict-Taxonomy-Canonical.md)
- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [CDM CtxL Canonical](../../fcdm-core/theory/cdm/Specifications/CtxL/CtxL-Canonical.md)
- [CDM Context Canonical](../../fcdm-core/theory/cdm/Specifications/Context/Context-Canonical.md)
