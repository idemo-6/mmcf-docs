---
title: "MMCF: Versioning Governance Profile (CDM-aligned)"
date: 2026-03-03
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

---

## 3. CDM-aligned invariants

1. Идентичность сущности неизменна в пределах инкарнаций.
2. Одинаковый валидный event log -> одинаковая версия (детерминизм).
3. Каждый компонент версии должен иметь событие-основание.
4. Ручное изменение версии запрещено, кроме явно аудируемого override.

---

## 4. Требования к derivation

Минимум:
- верифицированный event log;
- правила валидности переходов (phase-gates);
- детерминированная процедура вычисления.

Формат версии задается source-of-truth документом домена. Для CDM/MMCF таким
источником является CDM versioning profile.

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

## 7. Нормативная база CDM

Source-of-truth документы:
- [CDM Versioning Profile](../../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [CDM Version Derivation Policy](../../fcdm-core/theory/cdm/Specifications/Versioning/Version-Derivation-Policy.md)

---

## 8. Нормативные ссылки

- [MMCF-Canonical](./MMCF-Canonical.md)
- [Claim-Maturity-Canonical](./Claim-Maturity-Canonical.md)
