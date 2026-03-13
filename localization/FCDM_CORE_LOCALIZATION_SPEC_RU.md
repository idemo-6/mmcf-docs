# Спецификация локализации: `fcdm-core`

Язык: RU
Статус: рабочий черновик
Область: `/Volumes/WORK/Project/idemo_docs/fcdm-core`
Обновлено: 2026-03-13

## 1. Роль репозитория

`fcdm-core` является единым теоретическим контуром `FROR + CDM + bridge`.

Это большой и глубокий corpus теоретических документов. Для него ожидается
полная локализация, а не выборочный перевод нескольких entry files.

## 2. Выбранный режим

Для `fcdm-core` принимается режим:

- `mirrored locale tree`

Это означает:

1. язык должен быть выделен как structural root;
2. дерево документов после language root должно оставаться одинаковым;
3. локализация должна поддерживать language-first browsing.

## 3. Основная структура

Основной corpus локализации:

- `theory/`

Целевая структура:

```text
fcdm-core/
  theory/
    ru/
      cdm/...
      fror/...
    en/
      cdm/...
      fror/...
```

Правило:

1. после `theory/<lang>/` относительный путь документа должен быть одинаковым;
2. имена файлов ниже language root не переводятся;
3. path identity документа определяется парой:
   - `locale root`
   - `stable relative path after locale root`

Пример:

```text
theory/ru/cdm/Specifications/ChangeFlow-6_v3.md
theory/en/cdm/Specifications/ChangeFlow-6_v3.md
```

## 4. Root-level документы

Небольшой набор root-level operational/legal files не требует отдельного
mirrored tree.

Для таких файлов используется co-located pair:

```text
README_RU.md
README_EN.md
CONTRIBUTING_RU.md
CONTRIBUTING_EN.md
```

`LICENSE`, `NOTICE`, git hooks, scripts и подобные machine-facing artefacts не
локализуются.

## 5. Migration rule

Поскольку репозиторий уже существует в monolingual form, допускается
переходный режим:

1. текущие monolingual paths временно остаются source layer;
2. новые localization families создаются уже по целевой mirrored схеме;
3. по мере миграции старые monolingual documents переводятся в
   `theory/<lang>/...`.

Важно:

после начала mirrored migration не следует плодить внутри `theory/` пары
типа `*_RU.md` / `*_EN.md`.

## 6. Source-of-truth policy

Для каждой document family должен быть явно различим:

1. `source_language`
2. `translation_role`
3. `translated_from_version`

По умолчанию:

1. канонический исходник остается source-of-truth;
2. перевод считается derived version;
3. canon-sensitive translation не становится авторитетной без отдельного
   review verdict.

## 7. Scope exclusions

Не входят в localization corpus:

1. `.githooks/`
2. `scripts/`
3. git metadata
4. служебные machine-facing config files

## 8. Operational implication

Если source document изменился, а translation отстал:

1. это образует localization delta;
2. delta может быть `park`, `defer` или `promote`;
3. translation sync не должен молча происходить в случайной ручной правке без
   trace.
