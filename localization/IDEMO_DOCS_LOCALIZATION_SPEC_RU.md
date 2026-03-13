# Спецификация локализации: `idemo-docs`

Язык: RU
Статус: рабочий черновик
Область: `/Volumes/WORK/Project/idemo_docs/idemo-docs`
Обновлено: 2026-03-13

## 1. Роль репозитория

`idemo-docs` является document-first repo для:

1. IS-формализации;
2. runtime architecture;
3. ICSS profiles;
4. materialization profiles;
5. vision -> runtime bridge.

В отличие от `mmcf-docs`, здесь почти весь meaningful corpus уже является
документами верхнего уровня, а не mixed repo с code-like assets.

## 2. Выбранный режим

Для `idemo-docs` принимается режим:

- `mirrored locale tree at repo root`

Это означает:

1. основной document corpus должен иметь language-first structure;
2. путь после language root должен оставаться одинаковым;
3. repo должен быть пригоден для широкого bilingual browsing.

## 3. Целевая структура

```text
idemo-docs/
  ru/
    icss/...
    intent-authoring/...
    intent-parser-profile/...
    materialization/...
    runtime/...
    vision/...
  en/
    icss/...
    intent-authoring/...
    intent-parser-profile/...
    materialization/...
    runtime/...
    vision/...
```

Ниже language root имена файлов не переводятся.

## 4. Root-level files

Небольшой набор root entry docs может использовать co-located form:

```text
README_RU.md
README_EN.md
ENGINEERING_COLLABORATION_RU.md
ENGINEERING_COLLABORATION_EN.md
```

`LICENSE`, `NOTICE`, git metadata и подобные файлы не локализуются.

## 5. Почему не co-located mode

Для `idemo-docs` не рекомендуется массовый режим:

```text
runtime/FOO_RU.md
runtime/FOO_EN.md
```

Причина:

1. corpus широкий и должен стать полноценно bilingual;
2. language-first browsing здесь полезнее, чем sibling pairing;
3. mirrored tree проще для масштабной локализации.

## 6. Vision vs normative docs

Внутри `idemo-docs` нужно различать:

1. vision documents;
2. runtime/specification documents;
3. profiles and materialization docs.

Перевод vision docs допустимо делать более свободно.
Перевод runtime/specification docs должен считаться более строгим derived layer,
пока не пройден отдельный review.

## 7. Transition rule

Пока repo еще живет в текущей monolingual структуре, допускается migration
phase:

1. существующие directories временно остаются source layer;
2. новые localization families создаются уже по mirrored locale scheme;
3. после стабилизации corpus monolingual paths должны быть переведены под
   locale roots.
