---
title: "MMCF: профиль разделения phase state и transition state"
date: 2026-03-12
tags: [MMCF, PhaseTransition, phase, state, profile]
status: profile-draft
---

# MMCF: профиль разделения phase state и transition state

## 1. Scope

Этот профиль фиксирует простое правило:

- состояние фазы `ChangeFlow`
- и состояние `PhaseTransition`

не должны смешиваться в одну ось.

Профиль нужен как bridge между:

1. каноническим `CDM`, где `CF-PT` является автономным процессом;
2. существующими инструментами, где phase-oriented UI легко скрывает `PT`;
3. будущими MMCF-native системами, где `PT` можно делать отдельной сущностью.

---

## 2. Основание

По канону:

1. `CF-PT` является отдельным процессом передачи, согласования и подготовки;
2. `CF-PT` имеет собственное время, состояние и `result`;
3. завершение фазы не равно автоматически закрытому переходу;
4. `CF-PT` может оставаться в `In-Transition`.

Следовательно:

- `phase state` и `transition state` не тождественны;
- UI или tool mapping не должны их сливать.

---

## 3. Концептуальная модель

Для одного `ChangeFlow` различайте три слоя:

1. `Phase state`
   Где сейчас находится поток: `CF1..CF6`.
2. `PT plan`
   Какие переходы ожидаются и какие из них нетривиальны.
3. `PT instance`
   Конкретный runtime-объект перехода между двумя фазами.

У `PT instance` есть как минимум:

1. `from_phase`
2. `to_phase`
3. `pt_class`
4. `pt_form`
5. `pt_scenario`
6. `pt_runtime_state`
7. `result`
8. `attempt_no`
9. `policy`
10. `owners / executor`

При этом удерживайте три независимые оси описания перехода:

1. `pt_class = unconditional | conditional`
2. `pt_form = inline | process-based`
3. `pt_scenario = event | approve | claim | approve+claim`

---

## 4. Нормативное правило

Инструментальная система совместима с MMCF только если она удерживает различие:

1. где находится поток;
2. какой переход сейчас активен;
3. в каком состоянии находится этот переход.

Недопустимо:

1. кодировать `PT` только названием следующей фазы;
2. считать, что phase status уже содержит весь transition state;
3. терять факт активного handoff, approval, claim-acceptance или retry внутри
   phase-oriented UI.

---

## 5. Допустимые tool mappings

Для не-native систем допустимы два класса отображения.

### 5.1 Secondary transition axis

`PT` остается secondary state model поверх flow-object.

Минимально различаются:

1. `Active PT`
2. `PT Runtime State`
3. `PT Form`
4. `PT Scenario`
5. `PT trace`

### 5.2 Explicit transition object

`PT` выносится в отдельный tool object:

1. subtask;
2. child issue;
3. linked work item;
4. иная явная transition entity.

Это предпочтительно там, где:

1. переход materially влияет на длительность работы;
2. у перехода есть собственный owner/assignee;
3. нужен review trail, retry trail или отдельный approval contour.

---

## 6. PT object не является ChangeFlow

Если инструмент использует отдельный объект перехода, то:

1. он не становится terminal issue;
2. он не несет собственного `CF5`;
3. он не несет собственного `CF6` как `ChangeFlow`;
4. он не создает новый атомарный цикл изменения.

Parent `ChangeFlow` остается единственной атомарной единицей работы.

---

## 7. Existing tools vs native systems

### 7.1 Existing tools

В существующих системах типа `Linear` предпочтительно:

1. сохранять phase status только для parent `ChangeFlow`;
2. выносить нетривиальный `PT` в отдельный explicit transition object;
3. не создавать transition object для каждого короткого успешного `event`.

Для distributed `CF` действует practical default:

1. если соседние фазы реально принадлежат разным агентам или контурам,
   соответствующий `PT` по форме operationally следует считать
   `process-based`;
2. materialization в отдельный explicit transition object остается отдельным
   пороговым решением и зависит от значимости bottleneck.

### 7.2 MMCF-native systems

В native системе `PT` рекомендуется делать отдельной сущностью со своими:

1. owners;
2. runtime state;
3. policy;
4. attempts;
5. refs и history.

---

## 8. References

- [MMCF-Operational-Gateway-Scenario-Profile](./MMCF-Operational-Gateway-Scenario-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](./MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Minimal-Working-Model](./MMCF-Minimal-Working-Model.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./delivery/MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [CDM PhaseTransition-CF](../../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
