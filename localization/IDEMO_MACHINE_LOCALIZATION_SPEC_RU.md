# Спецификация локализации: `idemo-machine`

Язык: RU
Статус: рабочий черновик
Область: `/Volumes/WORK/Project/idemo_docs/idemo-machine`
Обновлено: 2026-03-13

## 1. Роль репозитория

`idemo-machine` является private code repository for IDEMO runtime
implementation.

Это code-first repo.

## 2. Выбранный режим

Для `idemo-machine` принимается режим:

- `selective co-located docs only`

То есть:

1. локализуются только human-facing entry docs;
2. code tree не дублируется по языкам;
3. repo-level locale roots не создаются.

## 3. Допустимая область локализации

Допускается локализация:

1. `README`
2. `QUICKSTART`
3. `ROADMAP_ENGINEERING`
4. selected explanatory docs in `docs/`

Формат:

```text
README_RU.md
README_EN.md
QUICKSTART_RU.md
QUICKSTART_EN.md
```

## 4. Что не локализуется

Не локализуются:

1. `src/`
2. `schemas/`
3. `tests/`
4. `ci/`
5. machine-facing examples and config

Причина:

локализация code-facing tree будет создавать drift без достаточной выгоды.

## 5. Engineering note

Если отдельный explanatory doc из `docs/` нужен русскоязычной команде, его
лучше локализовать co-located pair рядом с исходником.

Но repo не должен превращаться в mirrored bilingual documentation tree.
