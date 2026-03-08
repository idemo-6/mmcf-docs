---
title: "MMCF: Legacy Term Mapping (INDEX/MMCF -> CDM/FROR Canon)"
date: 2026-03-05
tags: [MMCF, CDM, FROR, glossary, migration, canonical]
status: canonical-draft
---

# MMCF: Legacy Term Mapping

## 1. Scope

Документ фиксирует обязательную нормализацию legacy-терминов из `INDEX/MMCF`
для публикации в `mmcf-docs` и согласования с текущим каноном CDM/FROR.

---

## 2. Normative Rules

1. Канонические имена сущностей и фаз задаются документами CDM/FROR.
2. Legacy-термины допускаются только как исторические алиасы в миграционных
   примечаниях.
3. В основном тексте методологических документов legacy-имена запрещены.
4. Любая "lambda/редукционная" физическая интерпретация допустима только через
   FROR-канон и `FROR <-> CDM Bridge`.

---

## 3. Legacy -> Canon Mapping

| Legacy | Canon | Type | Rule |
|---|---|---|---|
| `ODS` | `CDM` | rename | Полная замена без сохранения legacy-имени в тексте. |
| `OmniCycle` | `CDM` | rename (historical alias) | В тексте канона не использовать, допускается в сноске "исторический алиас". |
| `рассинхронизация` | `конфликт` / `Delta` (`Δ`) | semantic normalization | Использовать через Intent/Context/Applicability, а не как отдельный runtime-тип. |
| `триггер` (как общий драйвер change) | `конфликт` / `trigger-policy` (только где канонически задано) | scoped replacement | Термин `trigger` допустим в документах политики (`MetaChangeFlow-Trigger-Policy`), но не как универсальная замена `конфликта`. |
| `collectData` | `collect` (`CF1`) | typo + alias normalization | Всегда нормализовать до `collect`; канон фазы — `CF1`. |
| `forcast` | `forecast` (`CF3`) | typo fix | Орфография исправляется обязательно. |
| `collect/analyze/forecast/decide/implement/evaluate` | `CF1..CF6` | notation normalization | Доменные имена остаются только как алиасы представления рядом с каноном. |
| `k как время` (без модели цены фиксации) | `tau` через `c_fix` | FROR alignment | Время/необратимость описывать через FROR-инварианты. |
| `lambda-термины` (без bridge) | FROR profile + bridge mapping | FROR alignment | Запрещены как самостоятельный канон MMCF. |

---

## 4. Phase Notation Policy

Базовая запись:

`CF = [CF1 => CF2 => CF3 => CF4 => CF5 => CF6]`

Допустимые подписи-алиасы:

- `CF1 (collect)`
- `CF2 (analyze)`
- `CF3 (forecast)`
- `CF4 (decide)`
- `CF5 (implement)`
- `CF6 (evaluate)`

Алиасы не заменяют каноническую нотацию.

---

## 5. Result Semantics Policy

1. `Result=0` используется только для `ApplicabilityFailure`.
2. Нулевая стоимость перехода (`FROR.0_no_cost_transition`) не равна `Result=0`.
3. Для no-cost переходов используется отдельная аналитическая метка.

---

## 6. Banned Terms in Canonical MMCF Texts

В основном тексте канонических документов MMCF запрещены:

- `ODS`
- `OmniCycle`
- `forcast`
- `collectData`
- "lambda-физика" без ссылки на FROR-документы

---

## 7. Migration Annotation Template

Если legacy-термин требуется оставить для исторической трассировки:

`Legacy: <term> -> Canon: <term> (reason: <rename|semantic|fror-alignment>)`

Пример:

`Legacy: OmniCycle -> Canon: CDM (reason: rename-historical-alias)`

---

## 8. Minimal Lint Checklist (manual/CI)

Проверки для миграции текстов:

1. Нет запрещенных терминов из раздела 6.
2. Нет фазовой записи только через алиасы без `CF1..CF6`.
3. Любая трактовка времени/необратимости содержит FROR-ссылку.
4. `Result=0` не используется вне `ApplicabilityFailure`.

---

## 9. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Legacy-INDEX-Migration-Plan](./MMCF-Legacy-INDEX-Migration-Plan.md)
- [CDM ChangeFlow-6 Canonical](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM CF-LC Evaluate](../../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [FROR <-> CDM Bridge](../../fcdm-core/theory/cdm/bridge/FROR_CDM_bridge.md)
- [FROR Main](../../fcdm-core/theory/fror/FROR_main.md)

