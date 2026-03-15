---
title: "MMCF Delivery: профиль каталога PT-шаблонов"
date: 2026-03-12
tags: [MMCF, delivery, PhaseTransition, template, Linear]
status: working-draft
---

# MMCF Delivery: профиль каталога PT-шаблонов

## 1. Назначение

Этот профиль задает practical mapping для `PT template catalog` в existing
tools, прежде всего в `Linear`.

Он строится поверх:

- [MMCF-PT-Template-Catalog-Profile](../operational/MMCF-PT-Template-Catalog-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)

---

## 2. Main principle

В `Linear` template catalog не materialize'ится как новый top-level object.

Практическая схема такая:

1. template catalog lives in docs / profile / future config layer
2. parent terminal issue stores selected template bindings
3. explicit `PTSubTask` stores runtime instantiation

---

## 3. Parent issue: PT planning layer

В parent terminal issue selected templates должны жить в блоке:

- `PT / Gateways`

### 3.1 Минимальная форма binding

Для каждого нетривиального или заранее известного перехода:

1. `from_phase -> to_phase`
2. `selected_template_id`
3. `materialization = implicit | explicit`
4. optional policy override

### 3.2 Пример

```md
## PT / Gateways
- Default PT scenario: event
- Transition templates:
  - forecast -> decide = committee-approval
  - implement -> evaluate = ai-authority-approval
  - decide -> implement = implicit event
- PT failure policy:
- Negative result policy:
- Approval refs / owners:
- Claim refs / evidence refs:
```

Parent issue не хранит весь template object. Он хранит только selected binding.

---

## 4. PTSubTask: runtime layer

Если переход materialize'ится явно, `PTSubTask` хранит runtime-relevant defaults
и actual runtime state.

### 4.1 Минимум для PTSubTask

1. `Template id`
2. `Template family`
3. `From phase`
4. `To phase`
5. `PT class`
6. `PT scenario`
7. `PT runtime state`
8. `Failure policy`
9. `Owner / executor / authority`
10. `Refs`

### 4.2 Suggested title

`PT: <from_phase> -> <to_phase> / <selected_template_id>`

Пример:

`PT: forecast -> decide / committee-approval`

### 4.3 Suggested body

```md
## PT Runtime
- Parent flow:
- Template id:
- Template family:
- From phase:
- To phase:
- PT class:
- PT scenario:
- PT runtime state:
- Failure policy:
- Owner / executor / authority:
- Refs:
```

---

## 5. Что не копировать целиком

Не нужно копировать весь template catalog:

1. внутрь parent issue
2. внутрь `PTSubTask`

Нужно хранить только:

1. selected template id
2. template family
3. runtime-relevant defaults
4. actual runtime values

---

## 6. Visibility principle

1. parent issue показывает, где находится flow и какие transitions запланированы
2. `PTSubTask` показывает, где сейчас живет explicit transition
3. labels остаются только visibility hints и не заменяют template system

---

## 7. Skill integration

Практическая цепочка для skills:

1. template catalog
2. selected binding on parent issue
3. explicit `PTSubTask` runtime instance

### 7.1 Intake

`linear-cf-intake`:

1. пишет template bindings в `PT / Gateways`
2. не materialize'ит PT без нужды
3. может сразу создать `PTSubTask`, если переход заранее known to be explicit

### 7.2 Advance

`linear-cf-advance`:

1. читает binding на parent issue
2. решает, превышен ли threshold explicit materialization
3. создает/обновляет `PTSubTask` через `linear-pt-subtask`

### 7.3 PT runtime

`linear-pt-subtask`:

1. materialize'ит selected template в runtime object
2. переносит only runtime-relevant defaults
3. обновляет `pt_runtime_state`

### 7.4 Close

`linear-cf-close`:

1. читает relevant PT verdict/policy из runtime layer
2. не invent'ит exit в обход blocking PT instance

### 7.5 Repeat

`linear-cf-repeat`:

1. переносит только PT intent
2. не клонирует старые `PTSubTask`

---

## 8. Existing tools vs native systems

### 8.1 Existing tools

`PTSubTask` остается best-effort bridge-объектом.

### 8.2 Native systems

В native system `PTSubTask` должен схлопнуться в first-class
`PhaseTransitionInstance`, а template catalog и bindings должны стать
first-class config/runtime objects.

---

## 9. References

- [MMCF-PT-Template-Catalog-Profile](../operational/MMCF-PT-Template-Catalog-Profile.md)
- [MMCF-Delivery-PhaseTransition-Gateway-Profile](./MMCF-Delivery-PhaseTransition-Gateway-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Codex-Skills-Profile](./MMCF-Delivery-Codex-Skills-Profile.md)
