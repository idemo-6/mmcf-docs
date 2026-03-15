---
title: "MMCF Delivery: профиль синхронизации локализации"
date: 2026-03-13
tags: [MMCF, delivery, localization, translation, documentation, profile]
status: working-draft
---

# MMCF Delivery: профиль синхронизации локализации

## 1. Область

Этот профиль задает delivery-правила для локализации документных корпусов,
где один язык является source-of-truth, а другие языки поддерживаются как
derived human-facing versions.

Профиль нужен, чтобы:

1. не ждать полной стабилизации всего корпуса перед началом перевода;
2. не превращать translation drift в неуправляемую редакционную мелочь;
3. связать локализацию с `DeltaRegistry`, `ChangeFlow`, versioning и task
   workflow;
4. развести режимы локализации для document-first и code-first repositories.

Профиль относится к delivery и не переопределяет:

1. каноническую модель `ChangeFlow`;
2. `DeltaRegistry` и promotion semantics;
3. state-derived versioning;
4. claim-governance applicability.

---

## 2. Базовый принцип

Локализация по умолчанию не является "постскриптумом" исходной задачи.

Если bilingual output не входил в scope исходного `ChangeFlow`, то после
изменения source document возникает новая `Delta`:

- translation отсутствует;
- translation устарела;
- translation частично расходится;
- translation больше не соответствует текущему canon/profile.

Следовательно:

1. source change и translation sync могут жить в одном `CF` только когда это
   было явно задано scope'ом;
2. во всех остальных случаях localization mismatch трактуется как downstream
   `Delta`;
3. эта `Delta` затем может быть `park`, `defer` или `promote` в новый work
   object.

---

## 3. Primary Objects

### 3.1 Source document

`Source document` — документ family, который является текущим нормативным
или operational source-of-truth.

Минимально различимы:

1. `source_doc_ref`
2. `source_language`
3. `source_version_ref`, если versioning применим

### 3.2 Localization family

`Localization family` — набор языковых версий одного и того же document
contour.

Минимально различимы:

1. `family_id` или stable path family
2. `source_language`
3. `target_languages`
4. `translation_normativity`

### 3.3 Translation role

Для каждой target version должен быть различим `translation_role`:

1. `derived`
2. `authoritative`
3. `deprecated`

По умолчанию translation role = `derived`.

### 3.4 Localization delta

`Localization delta` — наблюдаемое расхождение между current source document и
target language version.

Типовые классы:

1. `missing`
2. `partial`
3. `stale`
4. `blocked`
5. `deprecated`

---

## 4. Sync State

Для target translation рекомендуется различать:

1. `missing`
2. `partial`
3. `stale`
4. `in_sync`
5. `blocked`
6. `deprecated`

Где:

1. `missing` — перевода еще нет;
2. `partial` — перевод есть только для части family;
3. `stale` — source materially changed после последнего translation pass;
4. `in_sync` — translation соответствует текущему source в допустимой степени;
5. `blocked` — sync зависит от authority, canon, release или policy verdict;
6. `deprecated` — translation intentionally no longer maintained.

---

## 5. Localization Policy

Для каждой family рекомендуется фиксировать `localization_policy`:

1. `manual`
2. `immediate`
3. `batched`
4. `release_only`
5. `canon_gated`

Смысл:

1. `manual` — перевод только по явному решению;
2. `immediate` — после существенного source change translation delta быстро
   materialize'ится в работу;
3. `batched` — локализация собирается пачками;
4. `release_only` — перевод синхронизируется на release/snapshot boundary;
5. `canon_gated` — перевод требует отдельного canon/authority review.

---

## 6. Relation to Versioning

Localization profile не создает отдельный первичный versioning canon.

Если document family version-aware:

1. source version остается source-of-truth;
2. translation хранит `translated_from_version` или эквивалентную trace note;
3. translation drift определяется относительно source version snapshot.

Нельзя:

1. кодировать самостоятельную "версию перевода" как замену source version;
2. смешивать localization state и entity version в одном поле;
3. считать translation status заменой `ObservedVersion`, `Pre-CF version` или
   `Post-CF version`.

---

## 7. Relation to Claim Applicability

Режим локализации зависит от claim mode document family.

### 7.1 `claim-none`

Для обычных guides, templates, operational docs и user-facing material:

1. translation обычно допускает более легкий sync;
2. `evaluate` проверяет прежде всего смысловую и навигационную совместимость;
3. localization flow чаще всего является обычным documentation flow.

### 7.2 `claim-optional`

Если документ обычно не claim-centric, но содержит отдельные сильные
утверждения:

1. перевод требует явной проверки этих утверждений;
2. claim-bearing fragments не должны silently drift;
3. gateway may require `approve` or `claim` on selected transitions.

### 7.3 `claim-required`

Для canon-sensitive, scientific или теоретической документации:

1. translation по умолчанию остается derived, а не authoritative;
2. localization flow требует более строгого `evaluate`;
3. при существенных claims или normative mappings нужен как минимум `approve`,
   а часто `approve+claim`.

---

## 8. Work Object Selection

Localization mismatch не всегда должен materialize'иться в terminal issue.

Нормальная лестница такая:

1. source change создает localization `Delta`;
2. `Delta` регистрируется и получает triage;
3. по policy выбирается `drop | park | defer | promote`;
4. только `promote` создает materialized localization work unit.

При выборе work object:

1. одиночный translation pass по конкретной family -> terminal `ChangeFlow`;
2. program-level coordination across many repositories -> coordination issue
   или epic;
3. long-running approval/claim handoff for translation acceptance -> `PTSubTask`;
4. raw localization backlog mismatch -> еще не terminal issue.

---

## 9. Trigger Rules

Рекомендуемые trigger rules:

1. `done` по source flow может создавать localization delta;
2. `repeat` по source flow обычно не должен немедленно materialize'ить
   translation sync, если source contour еще нестабилен;
3. `final/delayed` обычно ведут к `park` или `defer`, а не к immediate
   translation flow;
4. material source version change может быть trigger для version-aware sync;
5. release boundary может быть trigger для `release_only`;
6. canon-sensitive document families могут требовать explicit authority verdict
   перед promotion.

---

## 10. Repository Modes

Локализация не обязана иметь один и тот же structural mode во всех repos.

Минимально различайте:

1. `mirrored locale tree`
2. `co-located localized files`
3. `selective docs-only localization`
4. `no authoring localization`

Repo-specific recommendations вынесены в:

- [ROOT_DIRECTORY_LOCALIZATION_INDEX_RU](../../localization/ROOT_DIRECTORY_LOCALIZATION_INDEX_RU.md)

---

## 11. Evaluate Requirements

Минимальный `evaluate` для localization flow должен проверять:

1. translation покрывает целевую source delta;
2. структурные ссылки и references не сломаны;
3. target document не вносит смысловую регрессию;
4. translation role и sync state зафиксированы явно;
5. для claim-bearing families не возник silent claim drift.

Типовые operational outcomes:

1. `+1` — translation sync materially achieved;
2. `0` — translation currently inapplicable or intentionally deferred;
3. `-1` — translation introduced semantic regression or invalid claim drift.

---

## 12. Anti-Patterns

Нежелательно или запрещено:

1. автоматически переводить весь corpus после любой source edit;
2. молча продолжать старый source `CF` translation work, если bilingual scope
   не был заявлен заранее;
3. смешивать translation status с entity version;
4. считать каждый translation mismatch немедленным terminal issue;
5. трактовать derived translation как authoritative canon без отдельного verdict;
6. навязывать один structural localization mode всем repositories.

---

## 13. Normative References

- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](../operational/MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)
- [Versioning-Canonical](../governance/Versioning-Canonical.md)
- [MMCF-Delivery-Documentation-Profile](./MMCF-Delivery-Documentation-Profile.md)
- [MMCF-Delivery-Scientific-Documentation-Profile](./MMCF-Delivery-Scientific-Documentation-Profile.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
- [ROOT_DIRECTORY_LOCALIZATION_INDEX_RU](../../localization/ROOT_DIRECTORY_LOCALIZATION_INDEX_RU.md)
