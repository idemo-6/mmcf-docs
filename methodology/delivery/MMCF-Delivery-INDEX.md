---
title: "MMCF Delivery: индекс"
date: 2026-03-12
tags: [MMCF, delivery, Linear, software, documentation, profile]
status: working-draft
---

# MMCF Delivery: индекс

## 1. Назначение

Этот раздел содержит прикладные профили delivery для управления работой над
программными продуктами и документацией в MMCF-совместимых операционных
системах.

Раздел намеренно вынесен отдельно от корневого слоя `methodology/`, потому что
документы этого раздела ожидаемо вырастут в более крупный корпус.

### 1.1 Рабочая языковая политика

На текущем этапе `methodology/delivery/` ведется как русскоязычный рабочий
корпус.

Параллельную англоязычную редакцию на стадии черновиков не поддерживаем,
чтобы не создавать расхождения между версиями и не удваивать стоимость
каждой правки.

Когда материал стабилизируется, отдельную англоязычную редакцию имеет смысл
собирать как отдельный синхронизированный снимок, а не как второй живой
основной источник истины.

---

## 2. Базовое отображение

Текущая онтология delivery такова:

1. `Project` -> родительский экземпляр `LifeCycle`.
2. `Epic` -> дочерний экземпляр `LifeCycle` относительно `Project`.
3. терминальная задача -> один `ChangeFlow`.
4. `Context`, `C_obs`, `C_coord`, `C_meta` -> отдельные оси исполнения, а не
   иерархические уровни рабочих объектов.

Это сохраняет различие между `LifeCycle` и `ChangeFlow` в операционной
практике.

---

## 3. Текущие документы

Для короткого пользовательского входа в versioning без чтения полного
канонического профиля см.:

- [Versioning Quick Guide](../../VERSIONING_QUICK_GUIDE_RU.md)

1. [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
2. [MMCF-Delivery-Linear-Planning-Profile](./MMCF-Delivery-Linear-Planning-Profile.md)
3. [MMCF-Delivery-Planning-CI-V-Bridge](./MMCF-Delivery-Planning-CI-V-Bridge.md)
4. [MMCF-Delivery-Linear-Versioning-Profile](./MMCF-Delivery-Linear-Versioning-Profile.md)
5. [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
6. [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
7. [MMCF-Delivery-Codex-Skills-Profile](./MMCF-Delivery-Codex-Skills-Profile.md)
8. [MMCF-Delivery-Software-Profile](./MMCF-Delivery-Software-Profile.md)
9. [MMCF-Delivery-Documentation-Profile](./MMCF-Delivery-Documentation-Profile.md)
10. [MMCF-Delivery-Scientific-Documentation-Profile](./MMCF-Delivery-Scientific-Documentation-Profile.md)
11. [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
12. [MMCF-Delivery-AppliedRules-Integration-Profile](./MMCF-Delivery-AppliedRules-Integration-Profile.md)

---

## 4. Следующие ожидаемые документы

Следующими ожидаемыми профилями в этом разделе являются:

1. профиль проверки и evidence для `evaluate`;
2. профиль метрик delivery и наблюдаемости;
3. профиль недельного обзора и эскалации.

Профиль подбора исполнителей и capability registry намеренно вынесен в
корневой слой `methodology/`, а не в `delivery`, потому что он не привязан к
одному tool-profile:

- [MMCF-Executor-Matching-and-Capability-Registry-Profile](../MMCF-Executor-Matching-and-Capability-Registry-Profile.md)

---

## 5. Нормативная база

- [MMCF-Canonical](../MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](../MMCF-Minimal-Working-Model.md)
- [MMCF-Operational-Work-Unit-Contract](../MMCF-Operational-Work-Unit-Contract.md)
- [MMCF-Operational-Gateway-Scenario-Profile](../MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Claim-Governance-Applicability-Profile](../MMCF-Claim-Governance-Applicability-Profile.md)
- [MMCF-Planning-Assignment-Capability-Boundary](../MMCF-Planning-Assignment-Capability-Boundary.md)
- [MMCF-Executor-Matching-and-Capability-Registry-Profile](../MMCF-Executor-Matching-and-Capability-Registry-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](../MMCF-Operational-Roles-and-Gateways.md)
- [CDM LifeCycle-6](../../../fcdm-core/theory/cdm/Specifications/LifeCycle-6_v2.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md)
- [CDM SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md)
- [CDM Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md)
