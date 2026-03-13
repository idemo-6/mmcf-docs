# Спецификация локализации: `codex-skills`

Язык: RU
Статус: рабочий черновик
Область: `/Volumes/WORK/Project/idemo_docs/codex-skills`
Обновлено: 2026-03-13

## 1. Роль директории

`codex-skills` в корне `idemo_docs` не является самостоятельным documented
source repository.

Его следует трактовать как local install/mirror workspace, а не как
source-of-truth corpus для authoring and localization.

## 2. Выбранный режим

Для этой директории принимается режим:

- `no authoring localization`

Это означает:

1. не создавать здесь постоянные localization trees;
2. не вести здесь вручную bilingual skill corpus;
3. не рассматривать эту директорию как primary target для localization skill.

## 3. Правило source-of-truth

Если skill локализуется или получает bilingual helper docs, это должно
происходить в source repo:

1. `mmcf-docs`
2. `mmcf-code`
3. другие реальные skill-owning repositories

Локальный install/mirror слой должен получать уже собранный результат, а не
становиться местом независимой эволюции языковых копий.

## 4. Допустимые исключения

Допустимы только краткоживущие generated artifacts или local helper notes,
если они не претендуют на source-of-truth статус.

Но это не должно оформляться как регулярная localization practice.
