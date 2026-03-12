---
title: "MMCF: Versioning Governance Profile (CDM-aligned)"
date: 2026-03-12
tags: [MMCF, CDM, versioning, governance, profile]
status: profile-draft
---

# MMCF: Versioning Governance Profile

> В MMCF versioning не образует отдельный первичный канон. Нормативным
> source of truth остается CDM state-derived versioning, а MMCF задает
> governance-правила его применения.

## 1. Назначение

Документ задает applied-level правила versioning для систем, проходящих
управляемые изменения по lifecycle/changeflow в контуре MMCF.

Базовые инварианты и derivation наследуются из CDM. MMCF определяет только
управленческое использование этих правил в своих профилях и документах.

---

## 2. Базовый принцип

Версия является функцией верифицированных переходов состояния, а не ручным
тегом.

$$
\text{EntityVersion} = f(\text{EventLog},\ \text{phase-gates},\ \text{identity})
$$

Для сущностей CDM/MMCF каноническая форма состояния остается:

`EntityVersion = EntityId@v.<inc>.<lc>.<cf>.<cfp>`

где:

- `inc` — индекс инкарнации;
- `lc` — индекс текущей фазы `LifeCycle`;
- `cf` — индекс текущего meaningful `ChangeFlow`;
- `cfp` — индекс текущей фазы `ChangeFlow`.

---

## 3. CDM-aligned invariants

1. Идентичность сущности неизменна в пределах инкарнаций.
2. Одинаковый валидный event log -> одинаковая версия (детерминизм).
3. Каждый компонент версии должен иметь событие-основание.
4. Сегмент `cf` обязателен и не может быть отброшен из канонической версии.
5. Ручное изменение версии запрещено, кроме явно аудируемого override.
6. Work item не образует собственную автономную версию, если сам не является
   carrier entity.

---

## 4. Семантика индекса `cf`

Сегмент `cf` имеет в MMCF двойной смысл:

1. это индекс текущего meaningful `ChangeFlow` данной сущности;
2. это coarse signal накопленной итеративной глубины изменения сущности.

Следовательно:

1. `CF#N` не является декоративной меткой;
2. `CF#N` должен быть согласован с сегментом `cf` в `EntityVersion`;
3. более высокий `cf` допустимо читать как признак более длинной истории
   осмысленных изменений.

При этом запрещено трактовать `cf` как:

1. линейную метрику сложности;
2. прямую оценку трудозатрат;
3. показатель качества исполнения команды.

Высокий `cf` может означать:

1. реальную сложность;
2. длинную corrective/repeat цепочку;
3. внешний churn контекста;
4. claim/gateway/governance overhead.

---

## 5. Требования к derivation

Минимум:
- верифицированный event log;
- правила валидности переходов (phase-gates);
- детерминированная процедура вычисления.

Формат версии задается source-of-truth документом домена. Для CDM/MMCF таким
источником является CDM versioning profile.

---

## 6. Граница: сущность vs work object

MMCF использует versioning только как отражение состояния сущности, а не как
нумерацию самих рабочих объектов.

Следовательно:

1. `Project`, `Epic` и terminal issue не получают отдельной автономной
   "версии work item";
2. эти объекты хранят только snapshot, window или trace versioned entity;
3. если work item не привязан к versioned carrier entity, versioning-block в
   нем можно не применять.

---

## 7. Отображение на `Epic`

Если `Epic` представляет дочерний `LifeCycle` конкретной сущности, он должен
рассматриваться как lifecycle-level holder version snapshot, а не как отдельная
версионная сущность.

Рекомендуемый минимум:

1. `EntityId`
2. `ObservedVersion`
3. `CurrentCFIndex`
4. `VersionScopeRef`, если carrier не одна сущность, а bundle/manifest
5. `ClaimRegistryRef`, когда объект claim-bearing
6. `ExternalReleaseLabel`, если поверх state-derived version используется
   внешний release label

Нормативно:

1. `CurrentCFIndex` выводится из `ObservedVersion`, а не считается как сумма
   задач epic;
2. `Epic` не должен подменять `ObservedVersion` локальной нумерацией задач;
3. analytics уровня epic (`issue count`, `repeat count`, и т.д.) не являются
   заменой `CurrentCFIndex`.

---

## 8. Отображение на terminal `ChangeFlow`

Terminal issue представляет ровно один `ChangeFlow` carrier entity.

Поэтому issue хранит не "свою версию", а version window данного прохода:

1. `CarrierEntityId`
2. `CFIndex`
3. `InputVersion`
4. `PostCFVersion`
5. `VersionSourceRef`

Нормативно:

1. `CFIndex` issue должен соответствовать title-нотации `[CF#N]`;
2. `CFIndex` должен быть согласован с сегментом `cf` в `InputVersion` и
   `PostCFVersion`, когда они уже известны;
3. `InputVersion` фиксирует состояние сущности на входе в текущий `CF`;
4. `PostCFVersion` фиксируется только после `CF6` или помечается как
   `pending re-derivation`, если derivation еще не выполнена;
5. при `repeat` следующий issue наследует тот же `CarrierEntityId`, получает
   `CFIndex = N + 1` и использует `PostCFVersion` предыдущего issue как
   `InputVersion`, если она уже известна.

---

## 9. Override и аудит

Override версии допускается только в аварийном режиме и обязан содержать:
- `override_reason`
- `approved_by`
- `approved_at`
- `replay_plan`

После override обязателен возврат к state-derived версии.

---

## 10. Связь со статусами зрелости утверждений

`EntityVersion` и `ClaimStatus` — независимые оси governance.

- `EntityVersion` отвечает на вопрос: в каком процессном состоянии сущность.
- `ClaimStatus` отвечает на вопрос: какова зрелость конкретного утверждения.

Следовательно:

1. claim-bearing `Epic` может ссылаться на `ClaimRegistryRef`, но не должен
   кодировать `ClaimStatus` в `ObservedVersion`;
2. terminal issue может менять и `EntityVersion`, и claim-governance, но эти
   изменения фиксируются в разных полях/реестрах;
3. `ClaimStatus` не кодируется в `v.<inc>.<lc>.<cf>.<cfp>`.

---

## 11. Future automation

MMCF допускает future automation вокруг versioning, но она должна быть
derivation-oriented.

Практически это означает:

1. допустим validator, который проверяет согласованность `EntityId`,
   `CFIndex`, `InputVersion`, `PostCFVersion` и event/gate trace;
2. недопустим ручной registry version numbers без связи с derivation rules;
3. claim-style hooks могут служить governance-паттерном, но versioning-checker
   не должен быть простой копией claim registry validator.

---

## 12. Нормативная база CDM

Source-of-truth документы:
- [CDM Versioning Profile](../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [CDM Version Derivation Policy](../../fcdm-core/theory/cdm/Specifications/Versioning/Version-Derivation-Policy.md)
- [CDM Versioning Examples](../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Examples.md)

---

## 13. Нормативные ссылки

- [MMCF-Canonical](./MMCF-Canonical.md)
- [Claim-Maturity-Canonical](./Claim-Maturity-Canonical.md)
- [MMCF-Claim-Governance-Applicability-Profile](./MMCF-Claim-Governance-Applicability-Profile.md)
