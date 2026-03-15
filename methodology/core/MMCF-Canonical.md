---
title: "MMCF: Management Change Flow (CDM-Applied Methodology Draft)"
date: 2026-03-02
tags: [MMCF, CDM, ChangeFlow, methodology, applied-layer, governance]
status: working-draft
---

# MMCF: Management Change Flow (CDM-Applied Methodology Draft)

## 1. Назначение

MMCF задает не самостоятельную онтологию изменения, а прикладную
методологию управления изменениями поверх канона CDM.

MMCF не заменяет CDM и не переопределяет его инварианты. Его задача -
переводить канонические сущности CDM в управленческий контур planning,
governance, execution, delegation и delivery-интеграций.

MMCF также не заменяет существующие framework-level практики
(`Agile/Scrum/Lean` и др.), а задает прикладную рамку, внутри которой они
могут быть сопоставлены, ограничены и операционализированы без конфликта с
каноном CDM.

---

## 2. Область применения

Прикладной слой MMCF применим к:

1. бизнес-организациям и корпоративным процессам;
2. продуктовым и проектным изменениям;
3. технологическим и архитектурным системам;
4. когнитивным системам и ИИ;
5. социальным и образовательным процессам.

---

## 3. Нормативная база CDM

Source of truth для базовой семантики изменения находится в канонических
документах CDM:

- LifeCycle-6
- ChangeFlow-6
- PhaseTransition (LC/CF)
- ChangeOperators
- Context / CtxL
- Experience
- Versioning (state-derived)
- Claim Maturity (Conjecture/Protocol/Validated/Core)
- Subjectivity / Doubt / Viewpoint / MetaChangeFlow / CreativeChangeFlow
- Subjectivity Reference Agent Profile

Границы между CDM-каноном и applied-layer MMCF дополнительно фиксируются в:

- [MMCF-CDM-Alignment-Matrix](./MMCF-CDM-Alignment-Matrix.md)
- [MMCF-Applicability-Result-Alignment-Matrix](./MMCF-Applicability-Result-Alignment-Matrix.md)
- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](../operational/MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)

---

## 4. Базовые положения

1. Изменение начинается с обнаружимой `Delta`; управляемый проход по `CF1..CF6`
   materialize'ится только после promotion в active `ChangeFlow`.
2. Положение системы в `LifeCycle` определяет допустимую стратегию, риск и ресурсные пределы изменения.
3. Опыт фиксируется как динамический граф `flow -> context -> result`.
4. Решения должны быть причинно-трассируемыми и подтверждаться фазовыми гейтами.

---

## 5. Нормативные принципы MMCF как applied-layer

1. Полнота фаз `LifeCycle` и `ChangeFlow` обязательна.
2. Каждый `ChangeFlow` атомарен и завершается результатом по канону CDM.
3. Роли формализуются по уровням: системные, LC-фазовые, CF-фазовые.
4. Управление опирается на граф опыта, а не на ad-hoc эвристики.
5. Контекстная адаптация обязательна, особенно в режимах `develop` и `degrade`.
6. Делегирование допускается для всех фаз, кроме локального `implement`.
7. Принцип минимальной достаточности: только необходимый набор фаз/операторов.
8. Рекурсивность и самореференциальность: система модифицирует будущие циклы через опыт.
9. Эволюционная адаптивность: успешные операторы усиливаются, опасные подавляются.
10. Фокус на причинности: каждое действие фиксируется как `intent + context + result`.

---

## 6. Контур принятия решений

1. `Doubt` и `Subjectivity` допускаются как системные регуляторы выбора.
2. Оценка креативности выполняется по связке `R + S`, а не по `S` отдельно.
3. При неустойчивом выборе может запускаться `MetaChangeFlow` по trigger-policy.
4. Creative ChangeFlow допускается как специализированный режим,
   но не отменяет базовые phase-gates.

---

## 7. Операционный контракт MMCF

Минимальные обязательные поля для каждого materialized operational изменения
в контуре MMCF:

- `entity_id`
- `delta_ref`
- `intent_ref`
- `task_ref`
- `lc_phase`
- `cf_phase`
- `context_ref`
- `evidence_refs`
- `result` (`+1/0/-1`)

Запрещено проводить изменение без указания:

1. носителя действия;
2. места реализации;
3. наблюдаемого результата.

Pre-flow слой `Delta / DeltaRecord / DeltaRegistry / PromotionDecision`
определяется отдельным методологическим профилем и не редуцируется к одному
`task_ref`.

---

## 8. Версионирование

MMCF не задает отдельный от CDM первичный канон versioning.

В MMCF используется CDM-aligned state-derived versioning, а профиль MMCF
задает только правила управленческого применения и документирования.

Типовая проекция, допустимая в прикладных профилях:

`EntityId@v.<inc>.<lc>.<cf>.<cfp>`

Governance-правила применения задаются в:
- [Versioning-Canonical](../governance/Versioning-Canonical.md)

Source-of-truth для derivation и формата остается в CDM versioning profile.

Статус зрелости утверждений (`Conjecture/Protocol/Validated/Core`) ведется
как отдельная ось governance и не входит в числовые сегменты `EntityVersion`.
Профиль статусов см. в:
- [Claim-Maturity-Canonical](../governance/Claim-Maturity-Canonical.md)

---

## 9. Границы документа

Этот документ фиксирует applied-level рамку MMCF поверх CDM без попытки
переопределить каноническую онтологию, runtime-семантику или базовые
инварианты изменения.

Пороговые значения и доменные коэффициенты задаются профильными документами
конкретного контура внедрения.

---

## 10. Статус

Документ является рабочей корневой рамкой MMCF как applied methodology над
CDM.

Если меняются общие инварианты ChangeFlow/LifeCycle, source-of-truth должен
обновляться сначала в CDM, после чего в MMCF обновляется соответствующий
applied mapping.
