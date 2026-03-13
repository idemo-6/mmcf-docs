# Спецификация локализации: `mmcf-code`

Язык: RU
Статус: рабочий черновик
Область: `/Volumes/WORK/Project/idemo_docs/mmcf-code`
Обновлено: 2026-03-13

## 1. Роль репозитория

`mmcf-code` является private implementation/package layer:

1. installable Codex skills;
2. governance pipelines;
3. scripts;
4. tests;
5. operational setup docs.

Это code-adjacent repo, а не public methodology corpus.

## 2. Выбранный режим

Для `mmcf-code` принимается режим:

- `selective co-located docs only`

Это означает:

1. локализуются только human-facing documentation entrypoints;
2. code, scripts, tests и skill internals остаются English-first;
3. repo-level locale trees не создаются.

## 3. Что можно локализовать

Допустимая область локализации:

1. `README`
2. `COMMERCIALIZATION`
3. selected files in `docs/`

Формат:

```text
README_RU.md
README_EN.md
docs/CODEX_SKILLS_QUICKSTART_RU.md
docs/CODEX_SKILLS_QUICKSTART_EN.md
```

Если bilingual pair реально не нужен, файл можно оставить только в одном
исходном языке.

## 4. Что не локализуется

По умолчанию не локализуются:

1. `codex-skills/*/SKILL.md`
2. `codex-skills/*/agents/openai.yaml`
3. `scripts/`
4. `src/`
5. `tests/`
6. `governance-pipelines/`
7. CI config и другие machine-facing artefacts

Причина:

это operational instruction and execution layer, где удвоение языковых копий
будет давать больше drift-risk, чем пользы.

## 5. Public/private mirroring rule

Если human-facing guidance уже существует в `mmcf-docs`, не следует
автоматически дублировать ту же локализацию в `mmcf-code`.

Нужно различать:

1. public explanatory docs;
2. install/package-specific docs;
3. machine-facing skill internals.

## 6. Translation policy

Для `docs/` локализация допускается как derived helper layer.

Для skill files и automation internals translation by default не создается.
Если когда-либо появится необходимость bilingual skill authoring, это должно
быть отдельным design decision, а не обычной практикой.
