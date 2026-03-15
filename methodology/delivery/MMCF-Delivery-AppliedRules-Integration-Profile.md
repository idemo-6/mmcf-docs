---
title: "MMCF Delivery: профиль интеграции AppliedRules"
date: 2026-03-12
tags: [MMCF, delivery, AppliedRules, profile, decomposition]
status: profile-draft
---

# MMCF Delivery: профиль интеграции AppliedRules

## 1. Область

Этот короткий профиль объясняет, как онтология delivery в MMCF отображается в
CDM `AppliedRules`, не повторяя их полный контент.

Цель здесь только одна: обосновать delivery-декомпозицию:

1. `Project`
2. `Epic`
3. терминальная задача
4. `Context` и роли контекста

Семантика `PhaseTransition/PT` в этот профиль не входит; для нее используются
напрямую CDM `PhaseTransition-CF` и соответствующие MMCF gateway-профили.

---

## 2. `RLC/CC`

Используйте [RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md), чтобы определить, является ли кандидатный объект:

1. отдельным узлом, несущим собственный lifecycle;
2. или лишь процессом внутри родительского контура.

Следствие для delivery:

- `Project` и `Epic` используются только для контуров с собственным `LifeCycle`;
- терминальная задача трактуется как `ProcessOf(parent)` и представляет один
  `ChangeFlow`.

---

## 3. `ROS`

Используйте [ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md), чтобы остановить декомпозицию в точке, где дополнительная иерархия больше не добавляет:

1. объяснительной силы;
2. управленческой ценности;
3. устойчивости результата.

Следствие для delivery:

- не каждая рабочая тема становится `Epic`;
- декомпозиция останавливается на первом операционно достаточном уровне.

---

## 4. `SEC/OC`

Используйте [SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md), чтобы отделять внутреннюю декомпозицию от внешнего контекста.

Следствие для delivery:

- `Context` в этом профиле не моделируется как `Project` или `Epic`;
- внешние ограничения остаются в полях, метках и комментариях с evidence.

---

## 5. `Observer`

Используйте [Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md) для интерпретации ролей контекста.

Следствие для delivery:

- `C_obs`, `C_coord`, `C_meta` являются ролями исполнения/управления;
- это не иерархические уровни рабочих объектов.

---

## 6. Результат

Следовательно, онтология delivery такова:

1. `Project` -> родительский `LifeCycle`
2. `Epic` -> дочерний `LifeCycle`
3. терминальная задача -> один `ChangeFlow`
4. `Context` и роли контекста -> отдельные оси исполнения

---

## 7. Ссылки

- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Operational-Gateway-Scenario-Profile](../operational/MMCF-Operational-Gateway-Scenario-Profile.md)
- [CDM PhaseTransition-CF](../../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
- [CDM RLC-CC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md)
- [CDM ROS Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/ROS-Profile.md)
- [CDM SEC-OC Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md)
- [CDM Observer Profile](../../../fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md)
