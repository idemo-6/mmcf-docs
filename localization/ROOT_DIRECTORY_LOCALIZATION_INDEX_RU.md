# Спецификации локализации для корневых директорий `idemo_docs`

Язык: RU
Статус: рабочий черновик
Область: `idemo_docs`
Обновлено: 2026-03-13

## Назначение

Этот документ фиксирует базовую стратегию локализации для рабочих директорий
первого уровня в `/Volumes/WORK/Project/idemo_docs`.

Цель:

1. не смешивать разные режимы локализации в репозиториях разного типа;
2. сохранить стабильные path conventions;
3. подготовить основу для будущего localization-aware skill;
4. не ждать полной стабилизации всех документов перед запуском управляемой
   локализации.

`INDEX` исключен из этого набора намеренно.

## Рабочая область

В набор спецификаций входят:

1. `fcdm-core`
2. `mmcf-docs`
3. `mmcf-code`
4. `idemo-docs`
5. `idemo-machine`
6. `codex-skills`

Служебные директории `.obsidian` и `.nova` в localization corpus не входят.
Для них отдельная локализационная стратегия не требуется.

## Общие инварианты

Для всех каталогов в этой области действуют общие правила:

1. У одной document family должен быть ровно один source-of-truth язык в каждый
   момент времени.
2. Локализация не должна молча переписывать канон или source document.
3. Рассинхрон между source и translation трактуется как `Delta`, а не как
   случайная редакционная мелочь.
4. По возможности нельзя переводить сами path names; нужно сохранять стабильный
   structural path и разводить языки через naming или locale root.
5. Code, schemas, tests, scripts и machine-facing config по умолчанию не
   локализуются, если для этого нет отдельного сильного основания.
6. Для canon-sensitive и claim-bearing документов перевод по умолчанию является
   derived version, если иное не объявлено явно.

## Выбранные режимы по директориям

| Директория | Режим локализации | Короткий смысл |
|---|---|---|
| `fcdm-core` | mirrored locale tree | Полная локализация большого теоретического корпуса |
| `mmcf-docs` | co-located localized files | Частичная и асинхронная локализация публичных методологических документов |
| `mmcf-code` | selective co-located docs only | Локализуются только human-facing docs, не code/skills internals |
| `idemo-docs` | mirrored locale tree at repo root | Широкая локализация document-first runtime/formalization repo |
| `idemo-machine` | selective co-located docs only | Code-first repo, локализуются только docs entrypoints |
| `codex-skills` | no authoring localization | Локальный install/mirror слой, не source-of-truth корпус |

## Спецификации

1. [FCDM Core Localization Spec](./FCDM_CORE_LOCALIZATION_SPEC_RU.md)
2. [MMCF Docs Localization Spec](./MMCF_DOCS_LOCALIZATION_SPEC_RU.md)
3. [MMCF Code Localization Spec](./MMCF_CODE_LOCALIZATION_SPEC_RU.md)
4. [IDEMO Docs Localization Spec](./IDEMO_DOCS_LOCALIZATION_SPEC_RU.md)
5. [IDEMO Machine Localization Spec](./IDEMO_MACHINE_LOCALIZATION_SPEC_RU.md)
6. [Codex Skills Localization Spec](./CODEX_SKILLS_LOCALIZATION_SPEC_RU.md)

## Связь с будущим skill

Этот набор спецификаций нужен как structural basis для будущего
localization-aware skill.

Нормативный delivery-layer для этого набора теперь зафиксирован отдельно:

- [MMCF-Delivery-Localization-Sync-Profile](../methodology/delivery/MMCF-Delivery-Localization-Sync-Profile.md)

Ожидаемая логика skill:

1. определять document family и repo-specific localization mode;
2. проверять source language, translation role и sync state;
3. не материализовать translation work одинаково для всех репозиториев;
4. различать:
   - полную mirrored localization;
   - частичную co-located localization;
   - selective docs-only localization;
   - no-localization install mirrors.
