---
title: "MMCF: Context Example — Product and Engineering Platform"
date: 2026-03-05
tags: [MMCF, Context, Product, Engineering, Platform, examples, Applicability]
status: profile-draft
---

# MMCF: Context Example — Product and Engineering Platform

## 1. Назначение

Документ показывает прикладное применение контекстного канона MMCF для
software-команд на примере `Product` и `Engineering Platform`.

Цель:

1. выделять независимые контексты без смешения целей;
2. корректно решать межконтекстные конфликты;
3. связывать решения с `Applicable` и `Result`.

---

## 2. Контексты примера

### 2.1 `C_product_delivery`

Контекст продуктовой поставки:

1. фокус: пользовательская ценность и скорость релизов;
2. ограничения: roadmap commitments, UX, бизнес-сроки;
3. KPI: adoption, conversion, release cadence.

### 2.2 `C_platform_reliability`

Контекст платформенной инженерии:

1. фокус: надежность, масштабируемость, эксплуатационная устойчивость;
2. ограничения: SLO/SLA, архитектурные политики, capacity/security limits;
3. KPI: availability, incident rate, MTTR, infra cost efficiency.

---

## 3. Когда это два контекста

Вести как независимые контексты, если:

1. продукт требует ускорения релиза, а платформа требует hardening;
2. short-term KPI продукта конфликтует с reliability-policy платформы;
3. разные владельцы final decision и разные risk-толерансы;
4. `Applicable` может различаться по контекстам.

Типичный конфликт:

- Product хочет немедленно включить новую feature-флагом;
- Platform блокирует из-за невыполненных SLO-гейтов или риска деградации.

---

## 4. Когда допустим составной контекст

`C_product_platform_composite` допустим, если:

1. согласован общий KPI задачи (например, growth + reliability budget);
2. заранее согласованы release and reliability constraints;
3. назначен единый owner на задачу (или formal joint decision protocol);
4. возможен единый `Applicable`-gate.

Составной контекст действует только на конкретный flow.

---

## 5. Пример decision flow

Задача: \"Выпустить новый onboarding-flow в текущем релизном окне\".

1. `CF1/CF2`:
   - `C_candidates = {C_product_delivery, C_platform_reliability}`;
   - фиксируются бизнес и reliability ограничения.
2. `CF3`:
   - варианты: full release, phased rollout, shadow launch.
3. `CF4`:
   - `Applicable` проверяется в активном контексте.

Вариант A:

- phased rollout с лимитами нагрузки;
- `Applicable=true` в обоих контекстах при согласованных ограничениях.

Вариант B:

- full release без platform hardening;
- `Applicable=false` в `C_platform_reliability` -> `CF4 => CF6(inapplicable)` -> `Result=0`.

---

## 6. Интерпретация `Result=0` в примере

`Result=0` означает:

1. текущая траектория неприменима в заданных контекстных ограничениях;
2. а не \"команда не справилась\" как универсальный вывод.

После `Result=0`:

1. пересобрать ограничения (например, phased rollout);
2. сменить `C_active` при обосновании;
3. запустить новый ChangeFlow.

---

## 7. Operational checklist для Product/Platform

Перед запуском flow:

1. Совпадает ли целевая функция Product и Platform на этой задаче?
2. Согласованы ли release-критерии и SLO-гейты?
3. Кто owner финального решения при конфликте?
4. Можно ли поставить единый `Applicable`-gate?
5. Какие условия дадут `Result=0` и каков план компенсации?

Если на 2+ вопроса ответ \"нет\":

- вести задачу как мультиконтекстную с явной координацией.

---

## 8. Anti-patterns (пример)

Запрещено:

1. подменять reliability-гейты бизнес-срочностью без явного override-протокола;
2. считать platform-ограничения \"внутренним мнением\" вместо контекстных hard constraints;
3. трактовать `Result=0` как \"нейтральный релиз\".

---

## 9. Связь с каноном

Пример использует:

1. `CF1..CF6`;
2. `Applicable(Intent, C_active, LC_phase)`;
3. `Result=0 <=> ApplicabilityFailure`.

---

## 10. References

- [MMCF-Context-Canonical](./MMCF-Context-Canonical.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-DomainLexicon-Software-v1](./MMCF-DomainLexicon-Software-v1.md)

