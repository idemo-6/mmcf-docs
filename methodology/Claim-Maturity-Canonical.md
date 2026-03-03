---
title: "MMCF: Claim Maturity (Canonical)"
date: 2026-03-03
tags: [MMCF, claim-maturity, canonical, evidence, governance]
status: canonical-draft
---

# MMCF: Claim Maturity (Canonical)

## 1. Назначение

Документ задает каноническую модель зрелости научных/теоретических
утверждений для методологических и канонических документов MMCF/CDM/FROR.

Модель отделяет:
- версию сущности (`EntityVersion`, state-derived),
- зрелость утверждения (`ClaimStatus`).

---

## 2. Базовые объекты

- `ClaimId` — стабильный идентификатор утверждения.
- `ClaimText` — формулировка утверждения.
- `ClaimScope` — область применимости.
- `ClaimStatus` — текущий уровень зрелости.
- `EvidenceRefs` — ссылки на доказательства/протоколы/данные.

Статус является свойством утверждения, а не версии документа.

---

## 3. Каноническая шкала статусов

1. `Conjecture`
2. `Protocol`
3. `Validated`
4. `Core`

### 3.1 Conjecture

Минимум:
- явная формулировка утверждения;
- явные допущения;
- критерий возможного опровержения.

### 3.2 Protocol

Минимум:
- воспроизводимая процедура проверки;
- входы, метрики, критерий прохождения/провала;
- контрольные/анти-примеры.

### 3.3 Validated

Минимум:
- результаты по `Protocol`;
- ссылки на evidence (данные, журналы, скрипты, отчеты);
- отсутствие провала на обязательных контролях.

### 3.4 Core

Минимум:
- подтвержденная совместимость с действующим каноном;
- отсутствие конфликта с инвариантами и нормами;
- формальная канонизация в соответствующем Core/Criterion документе.

---

## 4. Переходы статусов (gate-policy)

Разрешены только последовательные переходы:

`Conjecture -> Protocol -> Validated -> Core`

Ручной skip-level переход запрещен.

Исключение: аварийный override с обязательным аудитом:
- `override_reason`
- `approved_by`
- `approved_at`
- `revalidation_plan`

---

## 5. Связь с versioning

`EntityVersion = EntityId@v.<inc>.<lc>.<cf>.<cfp>` описывает состояние
сущности в lifecycle/changeflow.

`ClaimStatus` описывает зрелость конкретного тезиса.

Эти оси независимы и не должны смешиваться в одном числовом сегменте версии.

---

## 6. Минимальный журнал событий статуса

Рекомендуемые события:
- `Claim.Created`
- `Claim.Revised`
- `Claim.ProtocolAttached`
- `Claim.Validated`
- `Claim.CoreAccepted`
- `Claim.Override`

Статус должен быть derivable из валидного event log.

---

## 7. Формат совместимости (Markdown + Obsidian)

Рекомендуется:
- держать `ClaimId` в документах (таблица/блок статусов);
- хранить детали и evidence в реестре (`claims_registry.yaml`);
- связывать документы wiki/markdown-ссылками.

---

## 8. Нормативные ссылки

- [MMCF-Canonical](./MMCF-Canonical.md)
- [Versioning-Canonical](./Versioning-Canonical.md)
- [CDM Versioning Canonical](../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [CDM Claim Maturity Profile](../../fcdm-core/theory/cdm/Specifications/Claim-Maturity-Profile.md)
