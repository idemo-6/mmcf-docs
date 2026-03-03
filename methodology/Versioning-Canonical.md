---
title: "MMCF: Versioning (Canonical)"
date: 2026-03-03
tags: [MMCF, versioning, canonical, state-derived, governance]
status: canonical-draft
---

# MMCF: Versioning (Canonical)

> Каноническое versioning в MMCF является **state-derived**, а не manual-tagged.

## 1. Назначение

Документ задает методологический канон versioning для систем, проходящих
управляемые изменения по lifecycle/changeflow.

Канон определяет принципы. Доменные форматы и события задаются профильными
документами (например, CDM profile).

---

## 2. Базовый принцип

Версия является функцией верифицированных переходов состояния, а не ручным
тегом.

\[
\text{EntityVersion} = f(\text{EventLog},\ \text{phase-gates},\ \text{identity})
\]

---

## 3. Инварианты канона

1. Идентичность сущности неизменна в пределах инкарнаций.
2. Одинаковый валидный event log -> одинаковая версия (детерминизм).
3. Каждый компонент версии должен иметь событие-основание.
4. Ручное изменение версии запрещено (кроме audit override).

---

## 4. Требования к derivation

Минимум:
- верифицированный event log;
- правила валидности переходов (phase-gates);
- детерминированная процедура вычисления.

Формат версии задается профильным документом домена.

---

## 5. Override и аудит

Override версии допускается только в аварийном режиме и обязан содержать:
- `override_reason`
- `approved_by`
- `approved_at`
- `replay_plan`

После override обязателен возврат к state-derived версии.

---

## 6. Связь со статусами зрелости утверждений

`EntityVersion` и `ClaimStatus` — независимые оси governance.

- `EntityVersion` отвечает на вопрос: в каком процессном состоянии сущность.
- `ClaimStatus` отвечает на вопрос: какова зрелость конкретного утверждения.

---

## 7. Профили применения

CDM-профиль versioning:
- [CDM Versioning Profile](../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [CDM Version Derivation Policy](../../fcdm-core/theory/cdm/Specifications/Versioning/Version-Derivation-Policy.md)

---

## 8. Нормативные ссылки

- [MMCF-Canonical](./MMCF-Canonical.md)
- [Claim-Maturity-Canonical](./Claim-Maturity-Canonical.md)
