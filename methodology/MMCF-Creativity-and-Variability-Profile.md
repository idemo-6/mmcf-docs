---
title: "MMCF: Creativity and Variability Profile (CI/V over CF1..CF6)"
date: 2026-03-05
tags: [MMCF, CDM, creativity, variability, profile, planning]
status: profile-draft
---

# MMCF: Creativity and Variability Profile

## 1. Scope

Документ задаёт прикладной профиль оценки сложности изменения через:

- `CI` (Creativity Index);
- `V` (Variability of alternatives).

Профиль используется для планирования времени/ресурсов по `CF1..CF6` и не
переопределяет канон `ChangeFlow-6`.

---

## 2. Canon Boundary

1. Канон фаз остаётся `CF1..CF6`.
2. `CI` и `V` — профильные параметры, а не канонические инварианты.
3. Решение в `CF4` и gate `PT(CF4->CF5)` обязательны независимо от `CI/V`.
4. `Result=0` остаётся только `ApplicabilityFailure`.

---

## 3. Definitions

### 3.1 Creativity Index (`CI`)

`CI` — индекс когнитивной/операционной новизны задачи в диапазоне `0..10`.

- `CI=0..3`: рутинная задача;
- `CI=4..7`: смешанный режим;
- `CI=8..10`: высоко-креативный режим.

### 3.2 Variability (`V`)

`V` — количество рассматриваемых допустимых альтернатив траектории до `CF4`.

Нормативно для профиля:

- используется конечное операционное множество `V_effective`;
- бесконечная вариативность в runtime не допускается.

---

## 4. Phase Impact Model

`CI` и `V` влияют на относительную нагрузку фаз:

1. `CF1/CF2`: растут с увеличением `CI`.
2. `CF3`: растёт с увеличением `CI` и `V`.
3. `CF4`: растёт с увеличением `V` (цена коллапса альтернатив).
4. `CF5`: относительная доля обычно снижается при высоком `CI`, но абсолютная
   стоимость определяется доменом и не выводится только из `CI`.
5. `CF6`: зависит от требований валидации и глубины фиксации опыта.

Рамочная запись:

- `T_CF3 ~ f(CI, V_effective)`
- `T_CF4 ~ g(V_effective)`
- `T_CF5 ~ h(domain_constraints, chosen_path)`

---

## 5. Planning Matrix (recommended)

Рекомендуемая матрица планирования:

1. Оценить `CI`.
2. Зафиксировать `V_effective` до старта `CF3`.
3. Проверить policy-ограничения `max_decide_delay`.
4. Назначить бюджет фазы по весам `w1..w6`, где:
   `sum(wi)=1`, `wi>=0`.
5. Пересчитать веса при изменении `V_effective` или контекста.

---

## 6. Safety Hooks (mandatory for creative mode)

Для задач с `CI >= threshold_creative` обязателен минимум:

1. `max_decide_delay` — верхняя граница задержки коллапса в `CF4`.
2. `forced_collapse_trigger` — условие принудительного перехода к
   `CF4`/`MetaChangeFlow`.
3. `resource_cap` — лимиты исследования альтернатив (`CF2/CF3`).

---

## 7. Minimal Data Contract

Минимальные поля профиля:

1. `task_id`
2. `ci_score`
3. `v_effective`
4. `cf_phase_weights`
5. `max_decide_delay`
6. `forced_collapse_trigger`
7. `assumptions_refs`

---

## 8. Anti-Patterns

Запрещено:

1. Использовать `CI` как замену результата (`Result`).
2. Бесконечно расширять альтернативы без перехода к `CF4`.
3. Смешивать канон (`CF1..CF6`) с неканоничными фазовыми именами.
4. Использовать `collectData`/`forcast` в новых канонических текстах.

---

## 9. Acceptance Checklist

1. Есть явная фиксация `CI` и `V_effective`.
2. Канонические фазы записаны как `CF1..CF6`.
3. Определены safety-hooks для creative mode.
4. Есть правило ограниченности вариативности.
5. Нет конфликтов с `Applicability` и `Result`-семантикой.

---

## 10. Normative References

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Term-Legacy-Mapping](./MMCF-Term-Legacy-Mapping.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [CDM ChangeFlow-6 Canonical](../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Creative ChangeFlow Canonical](../../fcdm-core/theory/cdm/Specifications/System/CreativeChangeFlow-Canonical.md)
- [CDM Creative ChangeFlow Model Core](../../fcdm-core/theory/cdm/Specifications/System/CreativeChangeFlow-Model-Core.md)

