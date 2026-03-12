---
title: "MMCF Delivery: профиль навыков Codex"
date: 2026-03-12
tags: [MMCF, delivery, Codex, skills, Linear]
status: working-draft
---

# MMCF Delivery: профиль навыков Codex

## 1. Назначение

Этот документ определяет первый проектный пакет навыков Codex для работы
с MMCF-совместимыми terminal `ChangeFlow` issues в Linear.

Навыки здесь являются операционными помощниками. Они не переопределяют сам
профиль delivery.

---

## 2. Пакет навыков v1

Начальный пакет содержит один входной навык и шесть узких навыков:

1. `linear-mmcf`
   Входной навык и маршрутизатор для MMCF-совместимой работы в Linear.
2. `linear-cf-intake`
   Создает новую терминальную задачу `ChangeFlow` под существующим `Epic`.
3. `linear-cf-advance`
   Продвигает терминальную задачу в следующую валидную фазу с учетом `PT`.
4. `linear-cf-close`
   Пишет сводку `CF6` и закрывает текущий поток с валидным terminal exit.
5. `linear-cf-repeat`
   Создает следующий соседний `ChangeFlow` issue из уже оцененного исходного потока.
6. `linear-pt-subtask`
   Создает или ведет explicit `PTSubTask` для нетривиального `PhaseTransition`
   под parent terminal flow.
7. `linear-version-sync-review`
   Проводит reconciliation `Epic`-уровня для `Observed version`,
   `Observed CF index` и `Version sync note`.

Такое разбиение сохраняет одну сильную точку входа, но при этом
оставляет под ней узкие операционные навыки.

Planning-profile `v1.1` считается аддитивным расширением этого пакета, а не
отдельным набором навыков.

---

## 3. Область каждого навыка

### `linear-mmcf`

Использовать, когда:

1. пользователь говорит о Linear через MMCF/CDM-понятия, а не только про
   общее управление тикетами;
2. задача может включать терминальный `ChangeFlow`, `PhaseTransition`, `CF6`,
   `repeat` или claim-aware delivery;
3. в начале хода еще неясно, какой именно узкий навык нужен.

### `linear-cf-intake`

Использовать, когда:

1. нужно создать новую терминальную задачу под существующим epic;
2. issue должен следовать правилам именования, шаблону body и правилам меток;
3. `Work Domain`, `Artifact Type` при необходимости, `Claim Mode` и ожидаемые
   нетривиальные `PT`-переходы уже известны или могут быть выведены.

### `linear-cf-advance`

Использовать, когда:

1. существующую терминальную задачу нужно перевести в следующую фазу;
2. необходимо обеспечить порядок фаз и правила `Applicable`;
3. узкие места, связанные с approval, claim или gateway, должны быть отражены
   в comments и labels.

### `linear-cf-close`

Использовать, когда:

1. issue находится в `evaluate`;
2. нужно оформить `CF6` с `Result`, `Failure class` и `PT` summary;
3. поток должен закрыться как `done`, `final` или `delayed`, либо быть
   подготовлен к `repeat`.

### `linear-cf-repeat`

Использовать, когда:

1. оцененный поток требует немедленного следующего `ChangeFlow`;
2. следующий issue должен унаследовать тот же контур и ту же базовую цель;
3. title, body, links и ссылки на `CF6` должны быть корректно перенесены.

### `linear-pt-subtask`

Использовать, когда:

1. нетривиальный `PhaseTransition` должен стать отдельным child object, а не
   только comment/label/body note;
2. переход требует собственного owner/assignee/history;
3. нужен explicit carrier для `approve`, `claim`, `approve+claim`,
   process-based handoff, retry или failure.

### `linear-version-sync-review`

Использовать, когда:

1. у `Epic` есть `Version sync note`;
2. `Observed version` выглядит stale относительно уже завершенных `CF6`;
3. нужен узкий reconciliation-pass по version snapshot, а не общий weekly review.

---

## 4. Общие операционные правила

Все навыки в этом пакете должны:

1. следовать delivery-документам в `methodology/delivery/`;
2. трактовать статус терминальной задачи только как фазовый слой;
3. не смешивать `phase state` и `transition state`;
4. при materially visible нетривиальном переходе использовать `PTSubTask`, а не
   притворяться, что `PT` является атрибутом фазы;
5. читать selected template binding на parent issue раньше, чем invent'ить ad hoc
   operational shape перехода;
6. не кодировать фазу, exit или активное узкое место в заголовке issue;
7. использовать labels только как вспомогательные сигналы видимости на уровне issue;
8. соблюдать правило `one terminal issue = one ChangeFlow`.
9. трактовать `PTSubTask` как explicit transition object, а не как второй
   terminal flow.
10. если workspace использует planning `v1.1`, навыки intake и repeat должны
   заполнять или сохранять task-side planning-поля и blocks, а advance/close
   не должны молча переписывать planning-assumptions.
11. versioning-aware навыки должны различать `Observed version` на уровне epic и
   `Pre-CF/Post-CF version` на уровне terminal issue.

Если Linear MCP не может напрямую выставить обязательное custom field, навык
должен:

1. все равно корректно создать или обновить body issue и comments;
2. явно упомянуть пропущенное обновление поля в финальной сводке.

---

## 5. Расположение исходников

Публичный versioned source of truth для этого профиля лежит в:

- `mmcf-docs/codex-skills/`

Приватное устанавливаемое зеркало и установщик живут в:

- `mmcf-code/codex-skills/`
- `mmcf-code/scripts/install_codex_skills.sh`
- `mmcf-code/scripts/check_codex_skills_sync.py`
- `mmcf-code/docs/CODEX_SKILLS_QUICKSTART.md`
- `mmcf-code/docs/LINEAR_MMCF_SETUP.md`

Целевой путь установки для живого использования Codex:

- `$HOME/.codex/skills/`

## 5.1 Sync policy

Между `mmcf-docs/codex-skills/` и `mmcf-code/codex-skills/` действует такая
дисциплина:

1. `mmcf-docs/codex-skills/` остается публичным versioned source of truth по
   смыслу и поведению навыков;
2. `mmcf-code/codex-skills/` является installable private mirror для реального
   развёртывания в Codex;
3. допустимое расхождение между ними ограничено packaging-layer деталями:
   bundled references, относительные vs installable link targets и похожие
   транспортные различия;
4. workflow/rules/metadata навыков не должны drift'овать между public source и
   private mirror;
5. изменение навыка считается завершённым только после sync обеих директорий.

Минимальный drift-check step:

```bash
python3 ../mmcf-code/scripts/check_codex_skills_sync.py
```

Checker сверяет:

1. одинаковый inventory навыков;
2. совпадение `name` и `description`;
3. совпадение нормализованного тела навыка после исключения reference-block
   различий.

---

## 6. Ссылки

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Delivery-PT-Template-Catalog-Profile](./MMCF-Delivery-PT-Template-Catalog-Profile.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
- [MMCF-Delivery-Linear-Planning-Profile](./MMCF-Delivery-Linear-Planning-Profile.md)
