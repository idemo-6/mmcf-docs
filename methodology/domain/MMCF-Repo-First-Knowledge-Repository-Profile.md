---
title: "MMCF: Repo-First Knowledge Repository Profile"
date: 2026-03-15
tags: [MMCF, repo-first, repo-reactive, knowledge-repository, claim-bearing, scientific-documentation, governance, profile]
status: working-draft
---

# MMCF: Repo-First Knowledge Repository Profile

## 1. Назначение

Этот документ собирает рабочую карту `Repo-First`/`Repo-Reactive` слоя для
`MMCF`-совместимых knowledge repositories.

Его задача не в том, чтобы объявить новый первичный канон поверх `CDM/MMCF`,
а в том, чтобы:

1. собрать в одном месте архитектурные принципы repo-first подхода;
2. связать их с уже существующими профилями `ClaimStatus`, `EntityVersion`,
   scientific-documentation delivery и repo-level governance pipelines;
3. зафиксировать, что именно уже существует в текущем стеке, а что еще
   остается будущим слоем.

Практически этот профиль нужен прежде всего для:

1. scientific-documentation repos;
2. canon-sensitive methodology repos;
3. claim-bearing research repos;
4. documentation-heavy repos, где значимы explicit registries, event logs,
   evidence refs и audit trail.

---

## 2. Базовое позиционирование

В текущем стеке полезно держать следующую иерархию:

`FROR` -> `CDM` -> `MMCF` -> `Repo-First repository profile` -> tools/interfaces

Где:

1. `FROR` задает часть онтологического и аналитического основания;
2. `CDM` задает каноническую модель изменения;
3. `MMCF` задает applied governance profile;
4. `Repo-First` задает инфраструктурный профиль хранения, трассировки и
   реактивной интерпретации knowledge-артефактов;
5. `Obsidian`, `Codex`, `CLI`, hooks, pipelines и будущие UI являются лишь
   операторами или проекциями этого слоя.

Следовательно:

1. `Repo-First` не подменяет `CDM/MMCF`;
2. `Repo-First` не является просто UI-паттерном;
3. `Repo-First` лучше понимать как repo-level governance and semantics profile
   для knowledge-bearing repositories.

### 2.1 Неконкурентное позиционирование относительно инструментов

`Repo-First` не следует подавать как замену привычным инструментам.

Корректнее говорить так:

1. он не требует отказаться от `Obsidian`, `Codex`, `CLI`, `VS Code`, git-hosting
   или issue-tracker;
2. он не объявляет один editor или один UI новым privileged tool;
3. он объединяет разные инструменты вокруг общего repository-centered workspace;
4. он добавляет общий semantics/governance layer поверх уже знакомых способов
   работы.

Следовательно, сообщение для принятия должно звучать не так:

1. “теперь у нас новый специальный инструмент вместо остальных”.

А так:

1. “у нас есть единое рабочее пространство артефактов”;
2. “разные инструменты работают над одним и тем же source of truth”;
3. “semantic state, claims, versions и audit trail не заперты внутри одного
   приложения”;
4. “переход между инструментами не разрушает целостность knowledge contour”.

### 2.2 Единое рабочее пространство

Практически `Repo-First` лучше всего позиционировать как общий рабочий слой,
который:

1. собирает документы, реестры, event logs, evidence refs и derived reports в
   один repo-centric contour;
2. позволяет разным участникам работать в привычных интерфейсах;
3. делает изменения сопоставимыми и аудируемыми в одной semantic frame;
4. позволяет future automation и AI-агентам работать не поверх разрозненных
   tool silos, а поверх единой структуры артефактов.

В этом смысле `Repo-First`:

1. не “ещё один editor”;
2. не “ещё один таск-трекер”;
3. не “ещё одна база знаний”;
4. а общий structural layer, который связывает их в одно рабочее пространство.

---

## 3. Когда этот профиль нужен

Использовать `Repo-First` профиль следует тогда, когда сам репозиторий хранит
значимое управляемое состояние, а не только тексты.

Типовые признаки:

1. в repo есть claim registry, version snapshot, event log или другой
   auditable governance state;
2. важны `canon/profile` references, а не только file contents;
3. требуется различать версию носителя и зрелость утверждения;
4. есть evidence layer, protocol layer или compatibility checks;
5. нужно, чтобы git/hook/pipeline могли блокировать неаудируемые изменения.

Типовые хорошие fit-сценарии:

1. научные и теоретические документы;
2. канонические и методологические профили;
3. validation/evidence пакеты;
4. documentation repos, где claims и version state являются частью
   operational governance.

Не стоит натягивать этот профиль на:

1. обычный low-ceremony backlog;
2. purely meta-operational docs без tracked governance state;
3. repos, где source of truth живет только во внешнем инструменте.

---

## 4. Ключевые инварианты

### 4.1 Repository as canonical state

Каноническим состоянием системы является репозиторий артефактов, а не
конкретное приложение, editor session или UI.

Короткая формула:

`SystemState = f(repository)`

### 4.2 Interfaces as reactive projections

Интерфейсы не владеют данными.

Они:

1. наблюдают изменения;
2. перестраивают индексы;
3. интерпретируют смысл;
4. визуализируют зависимости;
5. помогают оператору вносить новые изменения.

### 4.3 Determinism

Одинаковый валидный репозиторий должен приводить к одинаковому semantic state.

Это означает:

1. version/claim state должен быть derivable из явных артефактов;
2. hidden application state не должен становиться реальным source of truth;
3. derived artifacts должны быть воспроизводимы.

### 4.4 Non-ownership

Данные могут изменяться:

1. вручную через editor;
2. через `git pull`/merge/rebase;
3. через AI-агента;
4. через script/pipeline.

Это нормальный режим, а не ошибка архитектуры.

### 4.5 Governance at repo level

Governance применяется к артефактам и их трассе, а не к “сессии редактирования”.

Следовательно:

1. hook/pipeline проверяет repo artifacts;
2. commit message marker относится к auditable change;
3. override обязан иметь явный audit trail.

### 4.6 Независимость осей governance

`EntityVersion` и `ClaimStatus` должны оставаться раздельными.

Repo-first слой усиливает, а не отменяет эту развязку:

1. version state хранится и проверяется как отдельная ось;
2. claim maturity хранится и проверяется как отдельная ось;
3. один commit/flow может затрагивать обе оси, но не должен смешивать их.

---

## 5. Слои repo-first knowledge repository

### 5.1 Layer 1: Repository

Базовый слой содержит:

1. документы (`Markdown`);
2. реестры (`YAML`);
3. журналы событий (`YAML`/`JSON`);
4. governance-конфигурации;
5. явно зафиксированные derived artifacts, если они хранятся в repo.

Текущие примеры:

1. `claims_registry.yaml`;
2. `claim_event_log.yaml`;
3. `.mmcf/versioning/version_registry.yaml`;
4. `.mmcf/versioning/version_event_log.yaml`.

### 5.2 Layer 2: Change detection

Слой обнаруживает изменение репозитория.

Источники:

1. git hooks;
2. CI triggers;
3. file watchers;
4. manual refresh;
5. AI/script initiated changes.

Сейчас в стеке уже реально существуют:

1. `pre-commit`;
2. `commit-msg`;
3. `pre-push`;
4. portable repo-level governance pipelines.

### 5.3 Layer 3: Semantic index

Это слой производных представлений, которые помогают выйти за пределы file diff.

Типовые примеры:

1. `claim_index`;
2. `artifact_graph`;
3. `dependency_graph`;
4. `impact_report`;
5. `contradictions_report`;
6. localization drift reports.

Важный принцип:

1. semantic index может быть stored or generated;
2. он не должен молча становиться более каноничным, чем исходные артефакты;
3. если индекс хранится в repo, он должен считаться derived layer с понятной
   rebuild policy.

### 5.4 Layer 4: Interpretation and validation

Это слой, где file changes превращаются в semantic meaning и governance verdict.

Типовые операции:

1. claim extraction;
2. dependency reconstruction;
3. version continuity validation;
4. claim transition validation;
5. canon compatibility checks;
6. evidence completeness checks;
7. translation drift detection для derived translations.

### 5.5 Layer 5: Interfaces and operators

Сюда входят:

1. `Obsidian`;
2. `Codex`;
3. `CLI`;
4. scripts;
5. будущие knowledge UI/RRI.

Они не должны владеть состоянием.
Они работают как:

1. editors;
2. observers;
3. automation entry points;
4. visualization surfaces.

---

## 6. Минимальная модель артефактов

Для repo-first профиля недостаточно просто “иметь markdown-файлы”.
Нужно различать классы артефактов.

### 6.1 Canonical source artifacts

Это то, что считается первичным human/machine-readable source of truth.

Типовые классы:

1. теоретические и методологические документы;
2. claim registry snapshot;
3. claim event log;
4. version registry snapshot;
5. version event log;
6. governance configs и policy files.

### 6.2 Source artifacts with embedded semantic refs

Некоторые документы остаются первичными как тексты, но содержат ссылки на
отдельный governance state.

Типовые признаки:

1. `ClaimId` в тексте;
2. `EvidenceRefs`;
3. `ClaimRegistryRef`;
4. `VersionSourceRef`;
5. `canon/profile` refs;
6. explicit owner doc.

### 6.3 Derived artifacts

Это производные артефакты, которые допустимо materialize в repo, но нельзя
путать с первичным каноном.

Примеры:

1. `claim_index.yaml`;
2. `impact_analysis.md`;
3. `contradictions_report.md`;
4. sync reports;
5. notification snapshots.

### 6.4 External evidence artifacts

Не вся evidence обязана жить в том же файле, что и claim.

Допустимы:

1. reports;
2. datasets;
3. scripts;
4. experiment logs;
5. linked external artifacts.

Но если repo заявлен как repo-first для данного контура, то должна существовать
repo-visible trace:

1. что за evidence использована;
2. к какому claim/entity она относится;
3. где зафиксирован status-impact.

---

## 7. Минимальная модель зависимостей

Один из самых сильных тезисов repo-first набросков состоит в том, что система
должна понимать не только diff файлов, но и смысловое воздействие изменений.

Для этого нужен минимальный dependency vocabulary.

### 7.1 Рекомендуемые типы связей

Минимально полезны следующие relation types:

1. `governed_by` — документ управляется данным каноническим профилем;
2. `carries_claim` — документ несет формулировку или таблицу claim;
3. `status_recorded_in` — статус claim хранится в конкретном registry;
4. `evidenced_by` — claim опирается на evidence artifact;
5. `versioned_by` — carrier entity имеет version registry/event log;
6. `depends_on_concept` — документ зависит от канонического понятия;
7. `conflicts_with` — есть явный конфликт формулировок или норм;
8. `supersedes` — артефакт заменяет предыдущий;
9. `translated_from` — локализованный документ является derived translation;
10. `affects_scope` — изменение влияет на конкретный semantic scope.

### 7.2 Практический смысл dependency graph

Именно этот слой позволяет переводить:

1. `changed files = 3`

в более полезную форму:

1. `changed claim = 1`;
2. `affected docs = 12`;
3. `possible conflicts = 2`;
4. `recommended review actions = ...`.

### 7.3 Правило materialization

Dependency graph не обязан с первого дня жить в отдельной графовой базе.

На текущем этапе достаточно, если:

1. связи выражены явными refs в markdown/yaml;
2. часть графа derivable через checker/indexer;
3. derived graph при необходимости может быть экспортирован в файл.

---

## 8. Минимальная модель событий

Один из ключевых архитектурных вопросов repo-first слоя:

`что считается событием?`

Файловое изменение само по себе слишком слабое событие для claim-bearing
knowledge repo.

### 8.1 Три класса событий

#### A. Repository events

Низкоуровневые события:

1. file changed;
2. commit created;
3. branch merged;
4. push performed.

Они важны как trigger, но не задают semantic truth.

#### B. Governance events

Явные auditable события domain/governance уровня:

1. `Claim.Created`;
2. `Claim.ProtocolAttached`;
3. `Claim.Validated`;
4. `Claim.CoreAccepted`;
5. `Claim.Override`;
6. `Version.Snapshot`;
7. будущие version/compatibility override events.

Именно они должны определять state transitions там, где нужна строгая
аудируемость.

#### C. Derived semantic events

События, вычисляемые интерпретатором:

1. `claim_text_changed`;
2. `canon_compatibility_changed`;
3. `evidence_gap_detected`;
4. `translation_drift_detected`;
5. `contradiction_detected`;
6. `affected_artifacts_changed`.

Они полезны для review, subscriptions и impact analysis, но обычно не являются
самостоятельным каноном статуса.

### 8.2 Правило для state-bearing changes

Если изменение влияет на claim maturity, version state или другой audited
governance state, оно должно иметь:

1. machine-readable record;
2. разрешенную transition logic;
3. audit trail;
4. возможность checker-проверки до commit/push.

### 8.3 Override policy

Override допустим только как исключение.

Минимум:

1. `override_reason`;
2. `approved_by`;
3. `approved_at`;
4. `revalidation_plan` или `replay_plan`.

---

## 9. Repo-reactive processing model

Сильный практический тезис repo-first слоя:

`detect -> index -> interpret -> validate -> report/visualize`

### 9.1 Detect

Изменение обнаруживается через hook, CI, watcher или manual refresh.

### 9.2 Index

Система перестраивает производные semantic structures.

### 9.3 Interpret

Система отвечает не только на вопрос “какой файл изменился”, но и на вопросы:

1. какой claim или concept затронут;
2. какие документы и профили зависят от него;
3. есть ли возможный конфликт;
4. есть ли gap между текстом, registry и evidence.

### 9.4 Validate

На этом шаге применяются governance rules:

1. claim transition validity;
2. version continuity;
3. canonical compatibility;
4. scope and evidence checks;
5. translation/sync constraints, если релевантно.

### 9.5 Report / Visualize

Результат может проявляться как:

1. git rejection;
2. CI report;
3. derived markdown report;
4. CLI output;
5. UI alert;
6. future semantic subscription notification.

---

## 10. Collaboration model

Repo-first слой меняет саму единицу совместной работы.

### 10.1 Не document editing session, а change proposal

Базовая единица collaboration здесь:

1. commit;
2. branch;
3. PR;
4. auditable change set;
5. semantic review of affected artifacts.

### 10.2 Semantic change awareness

Хороший repo-reactive интерфейс должен показывать не только diff, но и impact.

Идеальная форма:

1. какие claims затронуты;
2. какие dependent artifacts требуют review;
3. какие compatibility checks стали обязательными;
4. какие subscriptions или owners должны быть уведомлены.

### 10.3 Continuous Theory Integration

Для scientific/canon-sensitive repos полезно читать этот цикл как
`Continuous Theory Integration`:

1. изменение теоретического артефакта;
2. semantic rebuild;
3. impact detection;
4. governance validation;
5. review affected artifacts;
6. merge / override / reject.

### 10.4 Semantic subscriptions

Одна из наиболее перспективных идей repo-first слоя - адресная подписка не на
файл как таковой, а на semantic scope.

Возможные уровни подписки:

1. branch-level;
2. artifact-scope;
3. concept-level;
4. claim-level;
5. domain-level.

Практический смысл:

1. если изменился конкретный claim, уведомляются только те, чьи артефакты
   реально зависят от него;
2. если изменился канонический concept, система может собрать dependent docs
   review list;
3. если возник conflict candidate, уведомление может идти не “всем”, а только
   relevant owners и reviewers.

На текущем этапе это лучше понимать как future derived layer поверх
dependency graph и semantic event model, а не как отдельный source of truth.

---

## 11. Что уже реально существует в текущем стеке

Этот слой важен не только как замысел. Некоторые его части уже materialized.

### 11.1 `fcdm-core`

Здесь уже есть strongest current example repo-first claim-governance:

1. `claims_registry.yaml`;
2. `claim_event_log.yaml`;
3. `check_claim_status_transitions.py`;
4. `.githooks/pre-commit`;
5. `.githooks/commit-msg`;
6. `.githooks/pre-push`.

Этот контур уже реализует:

1. auditable claim state;
2. separation of snapshot and event log;
3. commit-time enforcement;
4. explicit `claim:` marker requirement.

### 11.2 `mmcf-code`

Здесь уже есть portable repo-level enforcement package:

1. `claim-fror` pipeline;
2. `versioning` pipeline `v1`;
3. installer для `.githooks` и `.mmcf/governance/`.

Это особенно важно, потому что показывает:

1. pattern уже выносится из одного repo;
2. repo-first governance можно упаковывать как portable installable layer;
3. claim и version governance уже разведены по смыслу.

### 11.3 `mmcf-docs`

Здесь уже существуют необходимые соседние профили:

1. [Claim-Maturity-Canonical](../governance/Claim-Maturity-Canonical.md);
2. [Versioning-Canonical](../governance/Versioning-Canonical.md);
3. [MMCF-Minimal-Working-Model](../core/MMCF-Minimal-Working-Model.md);
4. [MMCF-Delivery-Scientific-Documentation-Profile](../delivery/MMCF-Delivery-Scientific-Documentation-Profile.md);
5. localization specs, где отдельно зафиксировано, что canon-sensitive и
   claim-bearing translations являются derived layer.

### 11.4 Что пока отсутствует как цельная система

Пока еще не materialized в общем виде:

1. generic semantic index layer для knowledge repos;
2. общий dependency graph protocol;
3. unified semantic impact engine;
4. subscriptions/notifications layer;
5. generic canon-compatibility checker;
6. единый repo-reactive UI.

Именно поэтому сейчас корректнее говорить не “система уже готова”, а
“архитектурный профиль уже виден и частично materialized”.

---

## 12. Как это лучше встроить в `mmcf-docs`

На текущем этапе правильнее всего держать этот материал именно в
`mmcf-docs/methodology` как дополнительный профиль.

Причины:

1. это еще не отдельный стабилизированный канон уровня `CDM`;
2. это уже больше, чем заметка о tool UX;
3. профиль напрямую касается scientific-documentation, claim-bearing docs,
   localization sync и repo-level governance pipelines;
4. `mmcf-docs` уже является естественным публичным applied-layer для таких
   профилей.

Следовательно, текущая рабочая формула:

1. `Repo-First` здесь живет как applied methodology profile;
2. delivery-профили используют его как structural background;
3. будущий более формальный infrastructure/runtime слой может быть выделен
   отдельно, когда появятся generic engine, schema и installable semantics.

---

## 13. Ближайшие шаги

Если развивать этот слой дальше, самый полезный следующий пакет работы такой.

### 13.1 Формализовать minimal artifact contract

Нужно жестко определить:

1. какие artifact classes считаются canonical;
2. какие считаются derived;
3. какие refs обязательны для claim-bearing/canon-sensitive docs.

### 13.2 Формализовать dependency vocabulary

Нужен минимальный список machine-readable relation types, который смогут
использовать:

1. docs;
2. registries;
3. checkers;
4. future indexers and subscriptions.

### 13.3 Формализовать event catalog

Нужно развести:

1. file-level triggers;
2. governance events;
3. derived semantic events.

Без этого semantic layer будет красивой метафорой, а не воспроизводимой
архитектурой.

### 13.4 Обобщить claim pipeline beyond FROR

Сейчас claim-governance strongest в `FROR`.
Следующий шаг - определить более общий claim-bearing repository profile,
который не будет зашит в конкретный теоретический корпус.

### 13.5 Определить storage policy для semantic indexes

Нужно решить по каждому классу derived artifacts:

1. хранится ли он в repo;
2. генерируется ли локально;
3. нужен ли ему commit;
4. как он синхронизируется и валидируется.

### 13.6 Добавить impact/reporting layer

Даже без full UI уже можно иметь полезные derived outputs:

1. affected claims report;
2. dependent docs review list;
3. contradiction candidates;
4. stale translation/registry warnings.

---

## 14. Короткий итог

`Repo-First` в контуре `MMCF` лучше всего понимать как профиль для
knowledge-bearing repositories, где:

1. repository является каноническим состоянием;
2. interfaces и agents являются реактивными операторами;
3. claim/version/evidence/governance state живет в явных repo artifacts;
4. semantic impact должен быть derivable из dependency и event layers;
5. scientific-documentation и claim-bearing docs являются естественной первой
   областью применения.

Именно в этом виде концепт уже достаточно созрел, чтобы быть не только
наброском в заметках, но и рабочим слоем внутри `mmcf-docs`.

---

## 15. Нормативные и опорные ссылки

- [Claim-Maturity-Canonical](../governance/Claim-Maturity-Canonical.md)
- [Versioning-Canonical](../governance/Versioning-Canonical.md)
- [MMCF-Minimal-Working-Model](../core/MMCF-Minimal-Working-Model.md)
- [MMCF-Domain-Pack-Profile](./MMCF-Domain-Pack-Profile.md)
- [MMCF_COMPATIBILITY_AND_ADOPTION_MODES](../../positioning/MMCF_COMPATIBILITY_AND_ADOPTION_MODES.md)
- [POSITIONING_AND_ADOPTION_ROADMAP](../../positioning/POSITIONING_AND_ADOPTION_ROADMAP.md)
- [MMCF-Delivery-Scientific-Documentation-Profile](../delivery/MMCF-Delivery-Scientific-Documentation-Profile.md)
- [MMCF-Delivery-Localization-Sync-Profile](../delivery/MMCF-Delivery-Localization-Sync-Profile.md)
- [CDM Claim Maturity Profile](../../../fcdm-core/theory/cdm/Specifications/Claim-Maturity-Profile.md)
- [CDM Versioning Canonical](../../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [FROR claims git workflow](../../../fcdm-core/theory/fror/FROR_claims_git_workflow.md)
- [Claim Governance Pipeline](../../../mmcf-code/docs/CLAIM_GOVERNANCE_PIPELINE.md)
- [Version Governance Pipeline](../../../mmcf-code/docs/VERSION_GOVERNANCE_PIPELINE.md)
