---
date: 2026-03-05
status: draft-plan
tags: [MMCF, CDM, FROR, migration, legacy, index-mmcf]
title: "MMCF: Legacy INDEX/MMCF Migration Plan (to CDM/FROR Canon)"
---

# MMCF: Legacy INDEX/MMCF Migration Plan

## 1. Scope

План покрывает нормализацию и внедрение материалов из `INDEX/MMCF/*` в
канонический корпус `mmcf-docs` с обязательной сверкой на:

- CDM-канон (`fcdm-core/theory/cdm/*`);
- FROR-канон (`fcdm-core/theory/fror/*`);
- мост `FROR <-> CDM` (`fcdm-core/theory/cdm/bridge/FROR_CDM_bridge.md`).

Цель: сохранить полезное содержимое legacy-текстов без переноса устаревших
терминов, нарушающих текущую каноническую нотацию.

---

## 2. Canonical Normalization Rules

### 2.1 Term Mapping (mandatory)

| Legacy term | Canonical replacement | Rule |
|---|---|---|
| `ODS` | `CDM` | Прямое переименование. |
| `OmniCycle` | `CDM` (исторический алиас) | В каноне не использовать как основное имя. |
| `рассинхронизация` | `Delta` (`Δ`) | Трактовать как первичный pre-flow объект несоответствия. |
| `триггер` (как первичный объект change) | `trigger-policy` | Использовать только как policy-signal, а не как первичный объект мира. |
| `конфликт` (как универсальное имя любой дельты) | `Delta` + qualification | Разводить `Delta` и квалифицированные conflict-cases. |
| `collect/analyze/forecast/decide/implement/evaluate` | `CF1..CF6` (или `C_1..C_6` как запись индексов) | Доменные имена допускаются только как алиасы представления. |
| `collectData` | `collect` (доменный алиас `CF1`) | Унифицировать написание. |
| `forcast` | `forecast` | Исправить орфографию и привязать к `CF3`. |
| lambda/редукционные термины (`beta-step`, `k` как время) | FROR-интерпретация (`tau`, `c_fix`, invariants I1..I6) | Любая физическая интерпретация только через FROR-канон. |

### 2.2 Result semantics (mandatory)

- `Result=0` разрешён только как `ApplicabilityFailure`.
- Нулевая цена перехода (`FROR.0_no_cost_transition`) не равна `Result=0`;
  фиксируется отдельной аналитической меткой (по bridge-политике).

### 2.3 Phase notation (mandatory)

Каноническая запись в новых документах:

`CF = [CF1 => CF2 => CF3 => CF4 => CF5 => CF6]`

Допустимые доменные алиасы:

- `CF1`: collect
- `CF2`: analyze
- `CF3`: forecast
- `CF4`: decide
- `CF5`: implement
- `CF6`: evaluate

---

## 3. Legacy Corpus Assessment

### 3.1 High-priority files (P1, сильное смысловое ядро)

1. `INDEX/MMCF/Конфликт.md`
2. `INDEX/MMCF/Время.md`
3. `INDEX/MMCF/**Субъективность.md`
4. `INDEX/MMCF/Фазовые переходы.md`
5. `INDEX/MMCF/Ролевая модель.md`
6. `INDEX/MMCF/Креативность.md`

### 3.2 Medium-priority files (P2, прикладные интерпретации)

1. `INDEX/MMCF/OODA.md`
2. `INDEX/MMCF/Без названия.md`
3. `INDEX/MMCF/Позиционирование.md`

### 3.3 Low-priority files (P3, редакционные/черновые)

1. `INDEX/MMCF/MMCF Draft.md`
2. `INDEX/MMCF/Принцип Apply-Only (Невозможность отката изменений).md`

---

## 4. Target Structure in mmcf-docs

### 4.1 New documents to create

1. `mmcf-docs/methodology/MMCF-Term-Legacy-Mapping.md`
2. `mmcf-docs/methodology/MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md`
3. `mmcf-docs/methodology/MMCF-Operational-Roles-and-Gateways.md`
4. `mmcf-docs/methodology/MMCF-Conflict-and-Applicability-Profile.md`
5. `mmcf-docs/methodology/MMCF-Creativity-and-Variability-Profile.md`
6. `mmcf-docs/methodology/MMCF-Time-and-Cost-FROR-Alignment.md`

### 4.2 Existing documents to update

1. `mmcf-docs/methodology/MMCF-Canonical.md`
2. `mmcf-docs/README.md`

---

## 5. Migration Plan (implementation waves)

### Wave 0: Lock glossary and invariants

1. Зафиксировать `Legacy -> Canon` словарь и anti-terms list.
2. Ввести lint-правила для запрета `ODS`, `OmniCycle`, `forcast`, `collectData`.
3. Зафиксировать обязательные ссылки на CDM/FROR bridge в новых профилях.

Deliverable:

- `MMCF-Term-Legacy-Mapping.md`.

### Wave 1: Conflict/Applicability normalization

1. Переписать legacy-семантику "триггер/рассинхрон" как Delta-first профиль.
2. Явно развести `Delta`, conflict qualification и policy-trigger.
3. Явно прописать `Result=0 <=> ApplicabilityFailure`.
4. Разделить runtime-исходы CDM, pre-flow слой MMCF и аналитические FROR-классы.

Deliverables:

- `MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md`.
- `MMCF-Conflict-and-Applicability-Profile.md`.

### Wave 2: Phases, roles, gateways

1. Нормализовать фазы на `CF1..CF6` (с доменными алиасами в скобках).
2. Вынести "фаза = роль" и "Gateway как отдельный контракт ответственности".
3. Привязать переходные сбои к `PhaseTransition`-слою, а не к фазам.

Deliverables:

- `MMCF-Operational-Roles-and-Gateways.md`.

### Wave 3: Creativity and variability formalization

1. Перенести CI/V-модель как domain-profile, не как канон.
2. Убрать неканоничные имена фаз (`collectData`, `forcast`).
3. Добавить критерии верификации и ограничения применимости.

Deliverables:

- `MMCF-Creativity-and-Variability-Profile.md`.

### Wave 4: Time and lambda semantics through FROR

1. Перевести "время как редукции" в FROR-совместимую форму (`tau`, `c_fix`).
2. Явно запретить самостоятельные "lambda-физические" утверждения без FROR-референса.
3. Развести: физическое утверждение, профильная интерпретация, гипотеза.

Deliverables:

- `MMCF-Time-and-Cost-FROR-Alignment.md`.

---

## 6. Acceptance Criteria

1. В `mmcf-docs` отсутствуют термины `ODS`, `OmniCycle`, `forcast`, `collectData`.
2. Все новые материалы используют `CF1..CF6` как базовую нотацию.
3. Каждая FROR-интерпретация ссылается на канонические FROR-документы.
4. Семантика `Result=0` не нарушает `ApplicabilityFailure`-правило.
5. Все migrated-документы имеют статус и нормативные ссылки.

---

## 7. Execution Queue (ordered)

1. Создать `MMCF-Term-Legacy-Mapping.md` (P1).
2. Создать `MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md` (P1).
3. Создать `MMCF-Conflict-and-Applicability-Profile.md` (P1).
4. Создать `MMCF-Operational-Roles-and-Gateways.md` (P1).
5. Создать `MMCF-Time-and-Cost-FROR-Alignment.md` (P1).
6. Создать `MMCF-Creativity-and-Variability-Profile.md` (P2).
7. Обновить `MMCF-Canonical.md` ссылками на новые профили (P1).
8. Обновить `README.md` (P1).

---

## 8. Out of Scope (for this plan)

1. Полная редактура всех legacy-файлов в `INDEX/MMCF`.
2. Формальная канонизация новых профилей в статус `core`.
3. Изменение CDM/FROR-канона без отдельного claim-процесса.
