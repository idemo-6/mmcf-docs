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
2. `ObservedVersion` или `InputVersion`
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
- Current CF index:
- Version scope ref:
- External release label:
- Claim registry ref:
- Version note:
```

Правила:

1. `Observed version` отражает последнее подтвержденное состояние сущности;
2. `Current CF index` выводится из `Observed version`, а не суммируется по
   задачам epic;
3. `Version scope ref` используется, когда carrier epic — это bundle или
   manifest, а не одна сущность;
4. `Claim registry ref` обязателен только для claim-bearing carrier.

---

## 5. Terminal issue как version window

Если terminal issue работает с versioned carrier entity, рекомендуется держать
в body:

```md
## Versioning
- Carrier entity id:
- CF index:
- Input version:
- Version source ref:
- Version intent note:
```

Правила:

1. `CF index` должен совпадать с title-формой `[CF#N]`;
2. `Input version` фиксирует состояние carrier entity на входе в текущий
   `ChangeFlow`;
3. `Version source ref` указывает на registry, документ, manifest или другой
   source of truth, из которого взят snapshot;
4. `Version intent note` описывает ожидаемую дельту состояния, а не ручной
   target version.

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

---

## 7. Правило для `repeat`

При `repeat` следующий issue должен наследовать:

1. тот же `Carrier entity id`;
2. `CF index = previous + 1`;
3. `Input version = previous Post-CF version`, если она уже известна;
4. ту же `Version source ref`, если source of truth не сменился.

Если `Post-CF version` предыдущего issue еще не выведена, новый issue может
временно использовать:

- `Input version: pending re-derivation from previous CF6`

---

## 8. Связь с claims

Для claim-bearing carrier entity действуют два правила:

1. `EntityVersion` и `ClaimStatus` остаются раздельными осями;
2. issue может одновременно обновлять version snapshot сущности и claim-layer,
   но не должен смешивать их в одном поле.

Следовательно:

1. `ObservedVersion/InputVersion/PostCFVersion` относятся к carrier entity;
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

1. считать `Current CF index` epic как сумму задач;
2. вводить отдельную ручную нумерацию версий для `Epic` или issue;
3. смешивать `ClaimStatus` с `ObservedVersion` или `Post-CF version`;
4. планировать ручной `target version` вместо intended delta;
5. обновлять version fields из-за labels/comments/admin changes в Linear.

---

## 11. Future automation

Позже допустим automation/checker слой, который проверяет:

1. согласованность `CF index` в title/body/version fields;
2. согласованность `InputVersion/PostCFVersion` с source of truth;
3. корректность повторной передачи version window при `repeat`.

Этот слой должен быть derivation-oriented и не должен превращать Linear в
ручной registry версий.

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
