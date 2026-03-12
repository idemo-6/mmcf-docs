---
title: "MMCF Delivery: bridge CI/V -> Flow Mode / Variativity Target / Decide Policy"
date: 2026-03-12
tags: [MMCF, delivery, planning, bridge, CI, V, Linear]
status: working-draft
---

# MMCF Delivery: bridge CI/V -> Flow Mode / Variativity Target / Decide Policy

## 1. Назначение

Этот документ задает короткий bridge между:

1. более общим методологическим профилем `CI/V`;
2. operational planning contour, используемым в `delivery/linear`.

Он нужен для того, чтобы старый аналитический язык не конфликтовал с новой
практической схемой issue-полей.

---

## 2. Базовое решение

В `delivery/linear` operational default теперь задается через:

1. `Flow Mode`
2. `Variativity Target`
3. `Decide Policy`
4. safety-hooks `max_decide_delay` и `forced_collapse_trigger`, когда они нужны

`CI/V` при этом не удаляется, но понижается до:

1. методологического planning-profile;
2. аналитического скаляра для оценки novelty/variability;
3. appendix-layer для legacy материалов и более общих planning discussion.

Нормативно:

1. `CI/V` не является default issue schema для `Linear`;
2. `delivery/linear` использует новый contour;
3. при наличии старого `CI/V` его нужно переводить в новый contour, а не
   тащить как параллельную обязательную issue-схему.

---

## 3. Правила перевода

### 3.1 `V_effective -> Variativity Target`

Прямое bucket-mapping:

1. `V_effective <= 1` -> `Variativity Target = 1`
2. `V_effective = 2` -> `Variativity Target = 2`
3. `V_effective = 3` -> `Variativity Target = 3`
4. `V_effective >= 4` -> `Variativity Target = 4+`

Это самое прямое соответствие между старыми и новыми planning fields.

### 3.2 `CI -> Flow Mode`

`CI` не переводится 1:1 в одно поле, но задает главный сигнал для `Flow Mode`.

Рекомендуемая шкала:

1. `CI = 0..3` -> обычно `Flow Mode = standard`
2. `CI = 4..7` -> `standard` или `creative` по явному planning rationale
3. `CI = 8..10` -> обычно `Flow Mode = creative`

При этом:

1. высокий `CI` не отменяет требования явно задать `Flow Mode`;
2. средний `CI` не должен автоматически переводиться в `creative` без
   объяснения, почему поток реально требует расширенного поиска.

### 3.3 `CI -> Decide Policy`

`CI` сам по себе не задает `Decide Policy`.

Правило:

1. `Decide Policy = delayed` используется только тогда, когда команда
   сознательно удерживает альтернативы открытыми;
2. высокий `CI` может объяснять `delayed`, но не имплицирует его автоматически;
3. если `Decide Policy = delayed`, это должно быть отражено отдельно, даже
   если `CI` уже высокий.

### 3.4 Safety hooks

Если старый материал использует высокий `CI` как признак creative mode, в
новом contour это должно переводиться не только в `Flow Mode=creative`, но и в:

1. `max_decide_delay`
2. `forced_collapse_trigger`
3. при необходимости `resource_cap`

То есть новый contour делает то, что старый `CI` намекал аналитически,
операционно явным.

---

## 4. Практическая матрица

| Старый сигнал | Новый operational contour |
|---|---|
| `CI=0..3`, `V=1` | `Flow Mode=standard`, `Variativity Target=1`, `Decide Policy=normal` |
| `CI=4..7`, `V=2..3` | `Variativity Target=2/3`; `Flow Mode` выбирается по rationale; `Decide Policy` задается отдельно |
| `CI>=8`, `V>=2` | обычно `Flow Mode=creative`; `Variativity Target` по bucket; safety-hooks обязательны |
| высокий `CI`, но быстрый collapse | `Flow Mode=creative`, `Decide Policy=normal` |
| высокий `CI` и сознательно отложенный выбор | `Flow Mode=creative`, `Decide Policy=delayed`, safety-hooks обязательны |

---

## 5. Правила для legacy материалов

Если в старом issue, документе или шаблоне уже используются `CI/V`:

1. не нужно сохранять `CI` как обязательный custom field в `Linear`;
2. переведите `V` в `Variativity Target`;
3. переведите creative-signal в `Flow Mode`;
4. задайте `Decide Policy` отдельно, а не выводите его молча из `CI`;
5. при необходимости сохраните исходный `CI` только как note в
   `Planning rationale` или как appendix outside issue schema.

---

## 6. Нормативный итог

1. `CI/V` остается валидным методологическим профилем.
2. `Flow Mode / Variativity Target / Decide Policy` является operational default
   для `delivery/linear`.
3. Новый contour не отменяет `CI/V`, а делает его delivery-перевод явным.

---

## 7. Ссылки

- [MMCF-Creativity-and-Variability-Profile](../MMCF-Creativity-and-Variability-Profile.md)
- [MMCF-Delivery-Linear-Planning-Profile](./MMCF-Delivery-Linear-Planning-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
