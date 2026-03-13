---
title: "MMCF: Domain Pack Profile"
date: 2026-03-12
tags: [MMCF, CDM, FROR, domain-pack, DomainLexicon, DomainMapping, adoption]
status: profile-draft
---

# MMCF: Domain Pack Profile

## 1. Назначение

Этот документ задает общий профиль `Domain Pack` для `MMCF`.

`Domain Pack` нужен для того, чтобы:

1. пользователь видел систему на языке своего домена;
2. канон `CDM/MMCF` сохранялся без размывания;
3. ИИ-агенты и automation продолжали работать на единой канонической схеме;
4. установка системы могла начинаться не с абстрактных `F1..F6` и `CF1..CF6`,
   а с понятного доменного интерфейса.

Идея проста:

- канон остается единым;
- домены получают свой внешний язык;
- `Domain Pack` связывает оба слоя.

---

## 2. Основание в CDM/FROR

Профиль опирается на уже существующие канонические принципы:

1. `DomainLexicon` в CDM задает отображение канонических фаз в доменную
   терминологию без изменения инвариантов.
2. `DomainMapping` в CDM/FROR задает доменные интерпретации, observables и
   примеры.
3. Доменные названия не определяют фазу и не меняют канон.

Референсы:

- [CDM DomainLexicon](../../fcdm-core/theory/cdm/DomainLexicon.md)
- [FROR DomainLexicon](../../fcdm-core/theory/fror/FROR_DomainLexicon.md)
- [CDM CF Domain: Management](../../fcdm-core/theory/cdm/DomainMapping/cf/CF-Domain-Management.md)
- [CDM LC Domain: Management](../../fcdm-core/theory/cdm/DomainMapping/lc/LC-Domain-Management.md)
- [CDM CF Domain: Computing](../../fcdm-core/theory/cdm/DomainMapping/cf/CF-Domain-Computing.md)
- [FROR Domain: Computing](../../fcdm-core/theory/fror/DomainMapping/fror/FROR-Domain-Computing.md)

---

## 3. Базовый принцип

`Domain Pack` не является альтернативным каноном.

Он является устанавливаемым доменным профилем, который:

1. сохраняет внутренние canonical ids;
2. подменяет наружный язык на доменный;
3. добавляет domain-specific примеры, observables и workflow defaults;
4. не меняет топологию `LifeCycle`, `ChangeFlow` и `PhaseTransition`.

Формула:

`CDM canon + MMCF applied layer + Domain Pack = domain-facing operational system`

---

## 4. Что остается каноном

Следующие элементы не должны настраиваться через `Domain Pack`:

1. шестифазная структура `LifeCycle`;
2. шестифазная структура `ChangeFlow`;
3. порядок фаз;
4. `CF5` как commit-point;
5. `CF6` как единственная точка фиксации `Result` и влияния `CF -> LC`;
6. `Result=0 <=> ApplicabilityFailure`;
7. различие между `Phase` и `PhaseTransition`;
8. entity-centric versioning;
9. отдельность `EntityVersion` и `ClaimStatus`;
10. базовые инварианты `FROR`.

Следовательно, внутренняя система, skills, pipelines и automation должны
продолжать оперировать на canonical ids:

- `F1..F6`
- `CF1..CF6`
- `LC-PT`
- `CF-PT`
- `Applicable`
- `Result`
- `EntityVersion`
- `ClaimStatus`

---

## 5. Что может менять Domain Pack

Через `Domain Pack` допустимо настраивать:

1. team-facing названия фаз;
2. пояснения фаз в языке домена;
3. доменные outputs/artefacts по фазам;
4. доменные observables;
5. типовые `PT` scenarios и bottlenecks;
6. templates для issue, `CF6`, review, version notes;
7. labels, views и onboarding wording;
8. human-facing wording для versioning и claim governance;
9. UI aliases и help text;
10. skill prompt overlays и install-time defaults.

Нормативно:

1. это доменная интерпретация, а не переопределение канона;
2. разные домены могут иметь разные слова при одном и том же canonical trace;
3. допустимы даже слияния терминов на внешнем UI-слое, но hidden canonical
   phases должны оставаться различимыми для automation и проверки.

---

## 6. Структура Domain Pack

Минимальный `Domain Pack` должен содержать следующие блоки.

### 6.1 Identity

1. `domain_id`
2. `domain_name`
3. `version`
4. `owner`
5. `status`

### 6.2 Canonical trace policy

1. явное подтверждение, что canonical ids сохраняются;
2. правило, что UI aliases не заменяют `F1..F6` и `CF1..CF6`;
3. правило, что все доменные шаблоны остаются маппируемыми в канон.

### 6.3 DomainLexicon

1. `LC lexicon`
2. `CF lexicon`
3. `PT lexicon`
4. `Result / Applicability wording`
5. `Versioning wording`
6. `Claim wording`, если релевантно

### 6.4 DomainMapping

1. observables по фазам;
2. типовые outputs;
3. типовые artifacts;
4. domain-specific bottlenecks;
5. типовые examples;
6. invalid/forbidden mappings

### 6.5 Delivery overlay

1. issue title convention
2. issue body template
3. `CF6` template
4. labels/views defaults
5. planning wording
6. versioning wording for `Epic` and terminal issue

### 6.6 Agent overlay

1. preferred prompts
2. domain aliases for skills
3. explanation style for users
4. default evidence wording
5. warning texts for anti-patterns

---

## 7. Install-time модель

Рекомендуемая схема внедрения:

1. выбирается базовый `Domain Pack`;
2. подтягиваются доменные названия фаз и словарь;
3. подтягиваются templates, labels, views и onboarding wording;
4. подтягивается skill overlay;
5. затем задаются workspace overrides под конкретную организацию;
6. canonical ids и canonical trace при этом не меняются.

Идея:

- пользователь работает в языке домена;
- ИИ и governance работают в каноне;
- между ними действует стабильный domain pack mapping.

---

## 8. Роль профильных специалистов

`Domain Pack` не должен делаться только методологом или только разработчиком.

Минимум нужны:

1. канонический owner, который защищает инварианты;
2. доменный специалист, который понимает реальный язык и observables;
3. delivery/tool owner, который понимает, как это проявляется в issue-tracker,
   шаблонах и automation;
4. при claim-heavy доменах — subject-matter reviewer.

Профильный специалист нужен не только чтобы “придумать красивые названия”.
Он нужен, чтобы определить:

1. какие observables реально различают фазу;
2. какие outputs считаются валидными;
3. где домен склонен ложно сливать разные канонические фазы;
4. какие gateway/approval patterns типичны;
5. как пользователю проще объяснять versioning и claims.

---

## 9. Валидация Domain Pack

`Domain Pack` считается пригодным, если:

1. все доменные фазы однозначно маппятся в canonical ids;
2. ни один alias не подменяет функциональную роль канонической фазы;
3. observables и outputs не противоречат канону;
4. templates и UI wording не ломают `Applicable`, `Result`, `CF6`, `PT`,
   versioning или claim-governance;
5. pack можно установить без потери canonical trace;
6. человек может работать в доменном языке, не изучая полный канонический
   словарь с нуля.

---

## 10. Рекомендуемые первые пакеты

Логичный стартовый набор:

1. `software`
2. `documentation`
3. `scientific-documentation`
4. `logistics`
5. `commerce`

Порядок разработки:

1. довести `software` до полноценного installable domain pack;
2. сделать второй нефайловый домен, чтобы проверить переносимость модели;
3. только после этого стабилизировать install-time domain selection.

---

## 11. Связь с текущими документами MMCF

Текущий документ задает общий профиль.

Частный пилотный пример уже существует:

- [MMCF-DomainLexicon-Software-v1](./MMCF-DomainLexicon-Software-v1.md)

Следующим слоем должны стать:

1. `MMCF Domain Pack: Software`
2. `MMCF Domain Pack: Logistics`
3. install/setup profile для выбора domain pack при развертывании tool-layer

---

## 12. Значение для восприятия проекта

`Domain Pack` — одна из ключевых user-facing частей всей связки `CDM/MMCF`.

Без неё проект легко воспринимается как:

1. слишком абстрактный;
2. перегруженный канонической терминологией;
3. трудный для входа без долгого обучения.

С ней проект может восприниматься иначе:

1. как система с глубокой теоретической базой;
2. как система, которая говорит с пользователем на языке его домена;
3. как система, где сложность удерживается каноном и ИИ-агентами, а не
   вываливается целиком на человека.

Именно поэтому `Domain Pack` надо считать не косметическим слоем, а одним из
основных механизмов принятия и масштабирования методологии.

---

## 13. Нормативные ссылки

- [MMCF-Canonical](./MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](./MMCF-Minimal-Working-Model.md)
- [MMCF-Term-Legacy-Mapping](./MMCF-Term-Legacy-Mapping.md)
- [MMCF-DomainLexicon-Software-v1](./MMCF-DomainLexicon-Software-v1.md)
- [POSITIONING_AND_ADOPTION_ROADMAP](../positioning/POSITIONING_AND_ADOPTION_ROADMAP.md)
- [CDM DomainLexicon](../../fcdm-core/theory/cdm/DomainLexicon.md)
- [FROR DomainLexicon](../../fcdm-core/theory/fror/FROR_DomainLexicon.md)
- [CDM CF Domain: Management](../../fcdm-core/theory/cdm/DomainMapping/cf/CF-Domain-Management.md)
- [CDM LC Domain: Management](../../fcdm-core/theory/cdm/DomainMapping/lc/LC-Domain-Management.md)
- [CDM CF Domain: Computing](../../fcdm-core/theory/cdm/DomainMapping/cf/CF-Domain-Computing.md)
- [FROR Domain: Computing](../../fcdm-core/theory/fror/DomainMapping/fror/FROR-Domain-Computing.md)
