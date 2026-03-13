# Спецификация локализации: `mmcf-docs`

Язык: RU
Статус: рабочий черновик
Область: `/Volumes/WORK/Project/idemo_docs/mmcf-docs`
Обновлено: 2026-03-13

## 1. Роль репозитория

`mmcf-docs` является публичным слоем методологии, delivery-профилей,
guides, templates, positioning и cases.

Этот repo уже содержит смешанный корпус:

1. canon-sensitive methodology docs;
2. public guides и templates;
3. user-facing positioning notes;
4. public skill mirrors.

Локализация здесь будет частичной и асинхронной.

## 2. Выбранный режим

Для `mmcf-docs` принимается режим:

- `co-located localized files`

Это означает:

1. семантическая структура repo остается первичной;
2. локализованные версии лежат рядом внутри той же папки;
3. repo-level `ru/` / `en/` trees не используются.

## 3. Naming convention

Используется одна basename family с language suffix:

```text
FOO_RU.md
FOO_EN.md
```

Примеры:

```text
guides/TASK_WORKFLOW_GUIDE_RU.md
guides/TASK_WORKFLOW_GUIDE_EN.md

CDM_MMCF_POSITIONING_RELATIVE_TO_SCRUM_KANBAN_OODA_RU.md
CDM_MMCF_POSITIONING_RELATIVE_TO_SCRUM_KANBAN_OODA_EN.md
```

## 4. Структурное правило

Локализованные файлы остаются внутри текущих semantic folders:

1. `guides/`
2. `templates/`
3. `cases/`
4. `positioning/`
5. `reports/`
6. `methodology/`
7. `localization/`

Причина:

для `mmcf-docs` важнее сохранить semantic browsing, чем language-first tree.

## 5. Skill and tooling boundary

Внутри `codex-skills/` действуют отдельные правила.

По умолчанию:

1. `SKILL.md` и `agents/openai.yaml` остаются English-first;
2. machine-facing instruction layer не дублируется по языкам без сильной
   необходимости;
3. при необходимости локализуются only references or public-facing helper docs,
   а не skill identity.

## 6. Canon sensitivity

Для `methodology/` и других canon-sensitive документов перевод по умолчанию:

1. является derived version;
2. не подменяет source text;
3. требует более строгого review, чем обычный guide/template.

Для `guides/`, `templates/`, `positioning/` и части `cases/` возможен более
легкий synchronization mode.

## 7. Trigger policy

Для `mmcf-docs` translation sync допустимо запускать:

1. после завершения source task;
2. после material version update relevant document family;
3. батчами по release или по cluster of related tasks.

Но translation sync не должен автоматически продолжать любой terminal `CF`,
если bilingual output не входил в его scope с самого начала.

## 8. Scope exclusions

Не нужно строить locale trees вида:

```text
ru/guides/...
en/guides/...
```

Это избыточно для текущего режима `mmcf-docs`.
