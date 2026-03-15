---
title: "Project Revision Report"
date: 2026-03-15
tags: [revision, project, mmcf, idemo, cdm, fror]
status: working-audit
---

# Project Revision Report

Дата: 15 марта 2026

## 1. Scope

Этот проход охватывает проект как составной workspace, а не только `MMCF`.
Проверка включала:

1. текущее состояние корневых entry points и инженерной навигации;
2. согласованность между `fcdm-core`, `idemo-docs`, `idemo-machine`,
   `mmcf-docs`, `mmcf-code`;
3. сопоставление с уже выпущенными ревизионными документами в
   `mmcf-docs/reports`;
4. оценку фактической implementability, а не только качества методологии.

База сравнения:

1. [MMCF Revision Report](./MMCF_REVISION_REPORT_2026-03-12.md)
2. [CDM/FROR Conflict Audit](./CDM_FROR_CONFLICT_AUDIT_2026-03-12.md)
3. [ENGINEERING_START_HERE](../../ENGINEERING_START_HERE.md)
4. [idemo-docs/README](../../idemo-docs/README.md)
5. [idemo-docs/ENGINEERING_COLLABORATION](../../idemo-docs/ENGINEERING_COLLABORATION.md)
6. [idemo-machine/QUICKSTART](../../idemo-machine/QUICKSTART.md)
7. [idemo-machine/ROADMAP_ENGINEERING](../../idemo-machine/ROADMAP_ENGINEERING.md)
8. [idemo-machine/docs/parser/IMPLEMENTATION_STATUS](../../idemo-machine/docs/parser/IMPLEMENTATION_STATUS.md)
9. [mmcf-docs/README](../README.md)
10. [mmcf-docs/engineering/ENGINEERING_COLLABORATION](../engineering/ENGINEERING_COLLABORATION.md)
11. [mmcf-code/README](../../mmcf-code/README.md)
12. [mmcf-code/docs/CODEX_SKILLS_QUICKSTART](../../mmcf-code/docs/CODEX_SKILLS_QUICKSTART.md)
13. [mmcf-code/docs/GOVERNANCE_PIPELINES_QUICKSTART](../../mmcf-code/docs/GOVERNANCE_PIPELINES_QUICKSTART.md)
14. [mmcf-code/docs/VERSION_GOVERNANCE_PIPELINE](../../mmcf-code/docs/VERSION_GOVERNANCE_PIPELINE.md)

Текущий repo snapshot по вложенным репозиториям:

| Repo | HEAD |
|---|---|
| `fcdm-core` | `a04a973` |
| `idemo-docs` | `3d052f2` |
| `idemo-machine` | `73f2d09` |
| `mmcf-docs` | `d490909` |
| `mmcf-code` | `d94fd44` |

Важно: корень workspace сам не является git-репозиторием.

## 2. Executive Summary

По состоянию на 15 марта проект уже выглядит как достаточно зрелый
theory-and-methodology stack с заметно более слабым engineering/runtime
слоем.

Сильное:

1. `CDM/MMCF/FROR`-контур стал заметно чище и согласованнее, чем в ревизии
   от 2026-03-12.
2. Несколько важных замечаний из предыдущего отчета уже закрыты:
   - конфликт source-of-truth в versioning устранен;
   - terminal closure для `Result=-1` теперь описан;
   - зависимость `PyYAML` и границы `Version Governance Pipeline v1`
     задокументированы явно;
   - public/private skill sync теперь не только описан, но и проверяется
     отдельным checker'ом.
3. `mmcf-docs` сейчас выглядит как наиболее цельный и быстро взрослеющий
   слой всего workspace.

Главный текущий риск проекта уже не в теоретической непротиворечивости, а в
том, что workspace в целом пока не имеет такого же строгого operational
контракта на уровне инженерной сборки и воспроизводимого project snapshot.

Проще:

1. теория и applied methodology продвинулись далеко;
2. инженерный контур пока в значительной степени scaffolding-first;
3. сборка проекта как единого продукта еще слабее, чем качество его частей.

## 3. Что изменилось относительно ревизии 2026-03-12

### 3.1 Закрытые или заметно ослабленные пункты

1. Вопрос про versioning source-of-truth закрыт в правильную сторону:
   - [CDM Versioning Canonical](../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
     теперь явно держит канон за `CDM`;
   - [MMCF Versioning Governance Profile](../methodology/governance/Versioning-Canonical.md)
     теперь явно позиционируется как governance/application layer.
2. Кейс terminal closure для `Result=-1`, отсутствовавший в старом отчете,
   теперь закрыт в:
   - [MMCF-Operational-Work-Unit-Contract](../methodology/operational/MMCF-Operational-Work-Unit-Contract.md)
   - [MMCF-Delivery-Terminal-ChangeFlow-Contract](../methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
   - [MMCF-Delivery-Linear-Profile](../methodology/delivery/MMCF-Delivery-Linear-Profile.md)
3. Ранее отмеченный documentation gap по `PyYAML` закрыт:
   - [GOVERNANCE_PIPELINES_QUICKSTART](../../mmcf-code/docs/GOVERNANCE_PIPELINES_QUICKSTART.md)
   - [VERSION_GOVERNANCE_PIPELINE](../../mmcf-code/docs/VERSION_GOVERNANCE_PIPELINE.md)
   - [CLAIM_GOVERNANCE_PIPELINE](../../mmcf-code/docs/CLAIM_GOVERNANCE_PIPELINE.md)
   - installer `install_governance_pipelines.sh` также валидирует зависимость.
4. Предыдущий риск drift между public и private skill package ослаблен:
   - есть [check_codex_skills_sync.py](../../mmcf-code/scripts/check_codex_skills_sync.py);
   - текущий sync check проходит успешно.

### 3.2 Что стало яснее

1. `mmcf-code` фактически оформился не как generic implementation repo, а как
   repo operational tooling:
   - installable skills;
   - git-hook governance pipelines;
   - setup docs и sync checks.
2. `idemo-machine`, наоборот, пока остается contract-first implementation
   placeholder с почти пустым `src/`.
3. `INDEX` по-прежнему огромен и полезен как internal memory/archive, но не
   является хорошим operational entry point для ревизии проекта как целого.

## 4. Current State by Layer

### 4.1 `fcdm-core`

Слой канона выглядит достаточно устойчивым и в этой ревизии не является
главным источником риска. После conflict audit и versioning alignment именно
`fcdm-core` выглядит наиболее нормативно определенным основанием всего
workspace.

### 4.2 `mmcf-docs`

Это сейчас самый зрелый слой проекта.

Плюсы:

1. хороший набор корневых методологических документов;
2. отдельные delivery-профили;
3. явный engineering entry point;
4. уже есть ревизионные документы, а не только канон.

По масштабу это уже не набор заметок, а реальный methodology corpus:

1. `29` файлов в `methodology/`;
2. `15` файлов в `methodology/delivery/`.

### 4.3 `mmcf-code`

Реальная полезность repo есть, но она уже довольно точно локализуется:

1. `codex-skills/` - пакет навыков;
2. `governance-pipelines/` - переносимые checker'ы и hooks;
3. `docs/` - инструкции и setup discipline.

При этом `src/` и `tests/` сейчас пусты. Поэтому repo полезен, но полезен
не как “implementation codebase MMCF”, а как packaging/tooling repo для
operational governance.

### 4.4 `idemo-docs`

Слой документации и контрактов выглядит содержательно сильным:

1. parser/runtime specs;
2. SQL materialization profiles;
3. vision bridge;
4. conformance corpus для intent parser profile.

Это хороший public contract layer, но его связь с `idemo-machine` пока
не доведена до полноценного engineering loop.

### 4.5 `idemo-machine`

Здесь разрыв наиболее заметен.

С одной стороны:

1. есть quickstart;
2. есть roadmap;
3. есть implementation status;
4. есть schema;
5. есть runtime-context test suite.

С другой стороны:

1. в `src/` найден только `src/parser/README.md`;
2. production parser/runtime code по сути отсутствует;
3. `tests/` содержит только README и suite для `runtime_context_binding`;
4. основной parser conformance corpus из `idemo-docs` сюда не перенесен.

Это не критично для research-stage проекта, но критично для честного
описания project readiness.

## 5. Findings

### [P1] У workspace нет единого воспроизводимого project baseline

Корень `/Volumes/WORK/Project/idemo_docs` не является git-репозиторием и не
содержит manifest/superproject-файла, который фиксировал бы согласованный
набор ревизий вложенных репозиториев.

Фактически проект существует как связка отдельных repo snapshots:

1. `fcdm-core`
2. `idemo-docs`
3. `idemo-machine`
4. `mmcf-docs`
5. `mmcf-code`

Проблема в том, что без workspace-level baseline ревизия “всего проекта”
остается partially reproducible: можно зафиксировать текущие HEAD'ы вручную,
но сам проект не хранит этот состав как собственный audited state.

Риск:

1. трудно воспроизводить ревизии и отчеты;
2. трудно понять, какие commit combinations считаются совместимыми;
3. меняется смысл фразы “проект в состоянии X”, потому что у project-level X
   нет встроенного носителя.

Рекомендация:

1. либо ввести superproject/manifest layer;
2. либо вести явный snapshot-документ с pinned SHAs и правилами sync между
   вложенными repos.

### [P1] `idemo-machine` не замыкает engineering loop на основной conformance corpus

Сейчас engineering repo декларирует conformance-first подход, но фактический
набор тестовых артефактов в нем уже уже, чем public contract corpus в
`idemo-docs`.

Наблюдаемое состояние:

1. в `idemo-docs/intent-parser-profile/runtime/tests/conformance/cases/`
   есть `8` parser conformance cases;
2. в `idemo-machine/tests/` нет зеркала этого корпуса;
3. в `idemo-machine/tests/` есть только runtime-context suite с `5` cases;
4. в `idemo-machine/src/` нет исполняемого parser skeleton.

Это делает инженерный цикл неполным:

1. public specs и fixtures уже существуют;
2. implementation repo на них ссылается;
3. но внутри implementation repo нет полного локального test surface,
   который позволял бы реально закрывать roadmap step-by-step.

Рекомендация:

1. импортировать или синхронизировать основной parser conformance corpus в
   `idemo-machine`;
2. добавить минимальный runner skeleton;
3. держать status matrix привязанной к реально присутствующим suite'ам.

### [P2] Корневые инженерные entry points устарели относительно текущего состояния проекта

Несколько файлов с пометкой `Source of Truth` и `Last Sync: 2026-03-06`
больше не описывают проект достаточно точно после расширения корпуса в
2026-03-12..2026-03-15.

Это касается как минимум:

1. [ENGINEERING_START_HERE](../../ENGINEERING_START_HERE.md)
2. [idemo-docs/ENGINEERING_COLLABORATION](../../idemo-docs/ENGINEERING_COLLABORATION.md)
3. [idemo-machine/QUICKSTART](../../idemo-machine/QUICKSTART.md)
4. [idemo-machine/ROADMAP_ENGINEERING](../../idemo-machine/ROADMAP_ENGINEERING.md)
5. [idemo-machine/docs/parser/IMPLEMENTATION_STATUS](../../idemo-machine/docs/parser/IMPLEMENTATION_STATUS.md)
6. [mmcf-docs/engineering/ENGINEERING_COLLABORATION](../engineering/ENGINEERING_COLLABORATION.md)

Проблема не только в дате. Проблема в том, что эти entry points не отражают:

1. появление ревизионных отчетов;
2. рост `mmcf-docs` methodology corpus;
3. текущую реальную роль `mmcf-code` как tooling repo;
4. неполный перенос conformance surface в `idemo-machine`;
5. состав nested-repo workspace.

Рекомендация:

1. обновить root onboarding files как navigation layer;
2. фиксировать не только “куда читать сначала”, но и фактическую maturity
   каждого подрепозитория.

### [P2] `mmcf-code` по описанию все еще звучит шире, чем его фактическое содержимое

По факту `mmcf-code` уже полезен, но его полезность сейчас почти полностью
лежит в skills, governance pipelines и operational docs.

При этом:

1. `src/` пуст;
2. `tests/` пуст;
3. README открывается формулировкой `Private code repository for MMCF implementation`.

Это создает expectation mismatch:

1. читатель ожидает implementation codebase;
2. реально получает tooling/package repo.

Риск невысокий, но прикладной:

1. это ухудшает onboarding;
2. затрудняет точную коммуникацию о зрелости проекта;
3. усиливает разрыв между методологической зрелостью и инженерными
   ожиданиями.

Рекомендация:

1. либо сузить позиционирование `mmcf-code` в README;
2. либо действительно начать наполнять `src/` и `tests/` тем, что repo
   обещает по названию и описанию.

## 6. Overall Assessment

Если оценивать проект как единый стек, то его зрелость сейчас неравномерна:

1. `fcdm-core` и `mmcf-docs` - strongest layer;
2. `mmcf-code` - useful tooling layer;
3. `idemo-docs` - strong public contract layer;
4. `idemo-machine` - still mostly implementation intention, not implementation.

Итоговая формула состояния:

1. theory-ready: да;
2. methodology-ready: в значительной степени да;
3. operational tooling-ready: частично да;
4. reproducible engineering-ready as a whole project: еще нет.

## 7. Priority Next Moves

1. Зафиксировать workspace-level baseline для всех вложенных repo.
2. Свести `idemo-docs` и `idemo-machine` в единый conformance loop.
3. Обновить root and repo-level onboarding/docs, которые помечены как
   `Source of Truth`.
4. Уточнить фактическое позиционирование `mmcf-code`.
5. Продолжать ревизии через `mmcf-docs/reports` как отдельный audit trail,
   не смешивая это с `INDEX`.

## 8. Short Conclusion

С 12 по 15 марта проект стал заметно лучше именно по качеству
самоопределения: теория выровнена, часть старых gaps закрыта, tooling
документация стала честнее.

Но новая главная проблема теперь другая: проект уже вырос до уровня, где
нужен не только сильный канон, но и собственный reproducible contract of the
workspace as a whole.
