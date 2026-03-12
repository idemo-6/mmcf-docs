---
title: "MMCF Delivery: профиль versioning для Linear"
date: 2026-03-12
tags: [MMCF, delivery, Linear, versioning, ChangeFlow, Epic]
status: profile-draft
---

# MMCF Delivery: профиль versioning для Linear

## 1. Область

Этот документ определяет, как entity-centric versioning отображается в Linear
для:

1. `Epic` как lifecycle-level holder;
2. terminal `ChangeFlow` issue как одного окна изменения.

Профиль не переопределяет CDM state-derived versioning и не заменяет
корневой governance-профиль:

- [Versioning-Canonical](../Versioning-Canonical.md)

---

## 2. Базовый принцип

В Linear не версионируется сам work item.
В Linear хранится только operational snapshot versioned entity.

Следовательно:

1. `Epic` не имеет отдельной "эпик-версии";
2. terminal issue не имеет отдельной "issue-версии";
3. оба объекта хранят только version snapshot или version window carrier entity.

---

## 3. Что должно быть видно в Linear

Для практики `v1` в Linear должны быть явно surface как минимум:

1. `EntityId`
2. `ObservedVersion` или `PreCFVersion`
3. `CFIndex`
4. `PostCFVersion`, когда `CF6` уже завершен

Дополнительно допустимы:

1. `VersionScopeRef`
2. `VersionSourceRef`
3. `ClaimRegistryRef`
4. `ExternalReleaseLabel`

В `v1` эти данные рекомендуется хранить в body issue и `CF6 Summary`, а не в
top-level custom fields.

---

## 4. `Epic` как lifecycle snapshot

Если `Epic` привязан к versioned carrier entity, рекомендуется держать в нем:

```md
## Versioning
- Entity id:
- Observed version:
- Observed CF index:
- Active CF issue:
- Version scope ref:
- External release label:
- Claim registry ref:
- Version note:
- Version sync note:
```

Правила:

1. `Observed version` отражает последнее подтвержденное состояние сущности;
2. `Observed CF index` выводится из `Observed version`, а не суммируется по
   задачам epic;
3. `Active CF issue`, если он виден, является отдельным operational marker, а
   не заменой `Observed version`;
4. `Version scope ref` используется, когда carrier epic — это bundle или
   manifest, а не одна сущность;
5. `Claim registry ref` обязателен только для claim-bearing carrier.
6. `Version note` описывает смысл текущего observed snapshot;
7. `Version sync note` описывает pending sync, re-derivation или
   reconciliation state.

### 4.1 Когда обновлять snapshot `Epic`

Для `delivery/linear` рекомендуется hybrid-policy:

1. `Observed version` обновляется event-driven, когда завершенный `CF6` уже
   дал явный новый snapshot;
2. создание нового issue `[CF#N]` само по себе не меняет `Observed version`;
3. переходы `collect/analyze/.../evaluate` сами по себе не меняют
   `Observed version`;
4. если `CF6` завершен, но `Post-CF version = pending re-derivation`, epic
   пока не обновляется;
5. в таких случаях допустимо указать в `Version sync note`, что sync pending
   from конкретного `CF#N`;
6. review, milestone или weekly sync служат reconciliation-point, но не
   обязаны быть единственным моментом обновления snapshot.

Практически это означает:

1. при `done` и `repeat`, если `Post-CF version` уже известна, `Epic`
   рекомендуется обновить сразу;
2. при `final` и `delayed` `Epic` обычно не обновляется, если
   `Version outcome note = no material version change`;
3. если `final/delayed` все же сопровождались material change и source of truth
   уже вывел новый snapshot, `Epic` можно обновить с явной note.

### 4.2 Как делить `Version note` и `Version sync note`

Используйте:

1. `Version note` для короткого описания того, что означает текущий observed
   snapshot, например:
   - `Observed snapshot after CF#12`
   - `Observed version aligned with stable registry snapshot`
2. `Version sync note` для operational состояния синхронизации, например:
   - `Sync pending from CF#13`
   - `Post-CF version pending re-derivation`
   - `Weekly reconciliation overdue`

Не смешивайте эти роли в одном поле.

### 4.3 Ритуал `version sync review`

Для `delivery/linear` полезен отдельный periodic reconciliation pass.

Scope:

1. только `Epic` с versioned carrier entity;
2. только `Epic`, где есть `Version sync note`, stale `Observed version` или
   явное расхождение между завершенными `CF6` и epic snapshot.

Цель:

1. проверить, появился ли уже валидный `Post-CF version` для pending flows;
2. обновить `Observed version` и `Observed CF index`, если snapshot уже ясен;
3. очистить, сократить или уточнить `Version sync note`;
4. при необходимости обновить `Active CF issue`.

Это reconciliation-ритуал, а не замена event-driven update policy.

---

## 5. Terminal issue как version window

Если terminal issue работает с versioned carrier entity, рекомендуется держать
в body:

```md
## Versioning
- Carrier entity id:
- CF index:
- Pre-CF version:
- Version source ref:
- Version intent note:
```

Правила:

1. `CF index` должен совпадать с title-формой `[CF#N]`;
2. `Pre-CF version` фиксирует последнее подтвержденное состояние carrier
   entity до входа в текущий `ChangeFlow`;
3. `Version source ref` указывает на registry, документ, manifest или другой
   source of truth, из которого взят snapshot;
4. `Version intent note` описывает ожидаемую дельту состояния, а не ручной
   target version.

### 5.1 Как читать это окно

`Pre-CF version` и `CF index` не обязаны указывать на один и тот же `cf`
сегмент.

Нормальная цепочка выглядит так:

1. предыдущий `CF#7` завершился на `...@v.1.2.7.6`;
2. новый issue `[CF#8]` получает `Pre-CF version = ...@v.1.2.7.6`;
3. если в ходе работы явно показывается текущий in-flow snapshot, он уже
   относится к `CF#8`;
4. после `CF6` issue фиксирует `Post-CF version = ...@v.1.2.8.6`.

Следовательно, `Pre-CF version` — это не "текущая версия внутри потока", а
унаследованный snapshot до входа в данный `CF`.

---

## 6. Закрытие `CF6`

В `CF6 Summary` для versioned carrier entity рекомендуется добавлять:

```md
- Post-CF version:
- Version outcome note:
```

Правила:

1. `Post-CF version` фиксируется только после завершенного `CF6`;
2. если derivation еще не выполнена, используйте `pending re-derivation`, а не
   угадывайте версию вручную;
3. `Version outcome note` кратко описывает, изменилась ли carrier entity
   materially, была ли зафиксирована новая версия и есть ли отложенное
   обновление snapshot.

### 6.1 Operational default по terminal exit

Для `delivery/linear` действует упрощающее operational правило:

1. новый material `Post-CF version` по умолчанию ожидается при `done`;
2. новый material `Post-CF version` по умолчанию ожидается при `repeat` для
   закрываемого `CF#N`;
3. при `final` и `delayed` с `Closure reason = inapplicable` по умолчанию
   используется `Version outcome note: no material version change`;
4. при `final` и `delayed` с `Closure reason = failed` `Version outcome note`
   определяется фактическим material change и не должен автоматически
   предполагать отсутствие изменения;
5. generic `final/delayed` сами по себе не являются правилом derivation
   версии и не подменяют source of truth.

Это правило введено для практики Linear и не является каноническим правилом
CDM version derivation.

Исключение допустимо, если:

1. carrier entity materially changed;
2. source of truth уже явно вывел соответствующий snapshot;
3. `CF6` кратко объясняет, почему несмотря на `final/delayed` фиксируется
   `Post-CF version`.

---

## 7. Правило для `repeat`

При `repeat` следующий issue должен наследовать:

1. тот же `Carrier entity id`;
2. `CF index = previous + 1`;
3. `Pre-CF version = previous Post-CF version`, если она уже известна;
4. ту же `Version source ref`, если source of truth не сменился.

Если `Post-CF version` предыдущего issue еще не выведена, новый issue может
временно использовать:

- `Pre-CF version: pending re-derivation from previous CF6`

---

## 8. Связь с claims

Для claim-bearing carrier entity действуют два правила:

1. `EntityVersion` и `ClaimStatus` остаются раздельными осями;
2. issue может одновременно обновлять version snapshot сущности и claim-layer,
   но не должен смешивать их в одном поле.

Следовательно:

1. `ObservedVersion/PreCFVersion/PostCFVersion` относятся к carrier entity;
2. `claim_ids_affected` и `claim_status_impact` относятся к claim-governance;
3. `ClaimRegistryRef` полезен на уровне epic, но не заменяет version fields.

---

## 9. Когда versioning block не нужен

Versioning block можно опустить, если:

1. задача meta-operational и не имеет versioned carrier entity;
2. задача purely procedural и не меняет состояние отслеживаемой сущности;
3. task работает только с governance-процедурой без отдельного carrier object.

В таких случаях не нужно симулировать версию искусственными значениями.

---

## 10. Anti-Patterns

Запрещено или нежелательно:

1. считать `Observed CF index` epic как сумму задач;
2. вводить отдельную ручную нумерацию версий для `Epic` или issue;
3. смешивать `ClaimStatus` с `ObservedVersion`, `Pre-CF version` или
   `Post-CF version`;
4. планировать ручной `target version` вместо intended delta;
5. обновлять version fields из-за labels/comments/admin changes в Linear;
6. использовать `Observed CF index` как индикатор активного потока вместо
   отдельного `Active CF issue`.

---

## 11. Future automation

Позже допустим automation/checker слой, который проверяет:

1. согласованность `CF index` в title/body/version fields;
2. логическую связность `PreCFVersion/PostCFVersion` с source of truth;
3. корректность повторной передачи version window при `repeat`.

Этот слой должен быть derivation-oriented и не должен превращать Linear в
ручной registry версий.

Текущий repo-level `Version Governance Pipeline v1` при этом следует читать
уже как continuity/snapshot checker, а не как полный CDM derivation replay.
Для `delivery/linear` этого достаточно как первого enforcement слоя, но не как
замены каноническому source of truth.

---

## 12. Ссылки

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Scientific-Documentation-Profile](./MMCF-Delivery-Scientific-Documentation-Profile.md)
- [Versioning-Canonical](../Versioning-Canonical.md)
- [Claim-Maturity-Canonical](../Claim-Maturity-Canonical.md)
- [MMCF-Claim-Governance-Applicability-Profile](../MMCF-Claim-Governance-Applicability-Profile.md)
- [CDM Versioning Canonical](../../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [CDM Version Derivation Policy](../../../fcdm-core/theory/cdm/Specifications/Versioning/Version-Derivation-Policy.md)
