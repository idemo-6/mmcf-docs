---
title: "CDM/FROR Conflict Audit"
date: 2026-03-12
tags: [MMCF, CDM, FROR, conflict, audit]
status: working-audit
---

# CDM/FROR Conflict Audit

## 1. Scope

Документ фиксирует audit остаточных употреблений термина `conflict` в
актуальном теоретическом корпусе `CDM/FROR` после принятия Delta-first схемы
в `MMCF`.

Цель audit:

1. различить допустимые conflict-cases от шумовых legacy-хвостов;
2. определить, где термин нужно сохранить, где переименовать, а где можно
   считать deprecated;
3. удержать source-of-truth за текущим `MMCF`-контуром, не переписывая
   теорию без реальной семантической необходимости.

---

## 2. Executive Summary

Итог audit на текущий момент:

1. В активном каноне `CDM` термин `conflict` в основном используется
   корректно:
   - как внутриконтекстное противоречие;
   - как межконтекстная или constraint-несовместимость;
   - как governance-коллизия правил/приоритетов.
2. В активном `FROR` термин `conflict` не является каноническим объектом;
   он либо обычное governance-слово, либо профильная метафора.
3. Явных живых мест, где `conflict` все еще претендует на роль universal
   starting object change, в `fcdm-core` не обнаружено.
4. В этом проходе найдены только два rename-case:
   - `CDM MetaChangeFlow Trigger Policy`
   - `FROR Electro Profile`
5. Полноценный deprecate-bucket для активного `CDM/FROR` сейчас фактически
   пуст.

---

## 3. Decision Matrix

| Path | Current use | Decision | Rationale | Action |
|---|---|---|---|---|
| `fcdm-core/theory/cdm/Specifications/CtxL/CtxL-Canonical.md` | внутриконтекстный конфликт и межконтекстная дивергенция | keep | это canonical conflict-case, а не универсальная `Delta` | без изменений |
| `fcdm-core/theory/cdm/Specifications/Context/Context-Canonical.md` | разрешение конфликтов между контекстами | keep | корректный coordination/context смысл | без изменений |
| `fcdm-core/theory/cdm/Specifications/Context/Context-Coordination-Protocol.md` | межконтекстные конфликты, конфликт приоритетов | keep | корректный coordination vocabulary | без изменений |
| `fcdm-core/theory/cdm/Specifications/Context/Context-Model-Core.md` | конфликт ограничений и приоритетов | keep | корректный `constraint-conflict` | без изменений |
| `fcdm-core/theory/cdm/Specifications/Context/Context-Multicontext-Applicability-Profile.md` | межконтекстный конфликт и conflict blocking | keep | корректный applied context-case | без изменений |
| `fcdm-core/theory/cdm/Specifications/Context/Context-Domain-Human-Multicontext-Profile.md` | конфликты между контекстами человека | keep | корректный доменный `context-conflict` | без изменений |
| `fcdm-core/theory/cdm/Specifications/Governance/Canonical-DSL-Governance.md` | конфликт терминов/ролей canon vs DSL | keep | governance-collision, не ontology of change | без изменений |
| `fcdm-core/theory/cdm/Specifications/AppliedRules/RLC-CC-Profile.md` | конфликт приоритета с каноном | keep | governance phrase only | без изменений |
| `fcdm-core/theory/cdm/Specifications/AppliedRules/Observer-Profile.md` | конфликт приоритета и политика разрешения конфликтов | keep | governance/context usage is valid | без изменений |
| `fcdm-core/theory/cdm/Specifications/AppliedRules/SEC-OC-Profile.md` | конфликты контекстов и приоритет у канона | keep | governance/context usage is valid | без изменений |
| `fcdm-core/theory/cdm/Specifications/System/System-Classification-Profile.md` | политика разрешения конфликтов | keep | ordinary governance/control wording | без изменений |
| `fcdm-core/theory/cdm/Specifications/System/MetaChangeFlow-Trigger-Policy.md` | рост конфликтности альтернатив | rename | здесь важнее несовместимость/коллизия альтернатив, а не общий `conflict` | переименовать в более точный термин |
| `fcdm-core/theory/fror/FROR_electro_profile.md` | потенциал конфликта/дефекта `Delta` | rename | `FROR` не нуждается в `conflict` как базовом объекте; здесь сильнее язык дефекта/несоответствия | переименовать в более точный термин |
| `fcdm-core/theory/fror/FROR_claims_git_workflow.md` | конфликт с действующим Core/Criteria | keep | обычный governance-термин | без изменений в этом проходе |
| `fcdm-core/theory/fror/FROR_experiment_registry.md` | конфликт инвариантов | keep | валидный экспериментальный критерий фальсификации | без изменений |
| `fcdm-core/theory/fror/FROR_architecture_v2.md` | отсутствие конфликта с действующим каноном | keep | обычная governance-формулировка | без изменений |

---

## 4. Keep Bucket

Термин `conflict` следует сохранять в `CDM`, когда он означает:

1. логическое противоречие в фиксированном контексте;
2. межконтекстную несовместимость;
3. конфликт ограничений;
4. конфликт authority/priorities/policies;
5. governance-коллизию между правилами или источниками канона.

Это соответствует принятой `MMCF` taxonomy:

- `context-conflict`
- `constraint-conflict`
- `authority-conflict`

А в ряде случаев также:

- `state-conflict`, если речь действительно о несовместимости `S_actual` и
  `S_ref` на одном carrier contour.

---

## 5. Rename Bucket

### 5.1 `MetaChangeFlow-Trigger-Policy`

Фраза про "рост конфликтности альтернатив" семантически слабее, чем
`несовместимость альтернатив` или `коллизия альтернатив`.

Проблема не критическая, но здесь `conflict` не добавляет ясности.

### 5.2 `FROR_electro_profile`

Фраза "потенциал конфликта/дефекта `Delta`" смешивает `FROR`-язык дефекта с
лишним словом `conflict`.

Для `FROR` сильнее термин:

- `дефект`
- `несоответствие`
- `напряжение`

В этом проходе выбран вариант без `conflict`.

---

## 6. Deprecate Bucket

В активном корпусе `CDM/FROR` на дату audit явных документов, где `conflict`
нужно формально объявлять deprecated как самостоятельный рабочий термин, не
обнаружено.

Deprecated-case фактически относится не к `fcdm-core`, а к historical layer:

1. `INDEX/MMCF/source/Конфликт.md`
2. старые формулы `конфликт (Delta)` в legacy MMCF

То есть deprecate-bucket сейчас лежит в legacy, а не в живом каноне
`CDM/FROR`.

---

## 7. Changes Applied In This Pass

В этом audit-pass применены только две теоретические правки:

1. `MetaChangeFlow-Trigger-Policy`:
   - `конфликтность альтернатив` -> более точная формулировка несовместимости
     альтернатив.
2. `FROR_electro_profile`:
   - `потенциал конфликта/дефекта` -> язык дефекта/несоответствия без
     лишнего `conflict`.

Причина минимализма:

- в `fcdm-core` уже есть unrelated user changes;
- audit должен менять только реально шумящие места;
- корректные context/governance conflict-cases ломать не нужно.

---

## 8. Short Conclusion

Текущий канон `CDM/FROR` в целом уже совместим с принятым Delta-first
контуром `MMCF`.

Главный вывод audit:

1. `Delta` остается базовым объектом изменения;
2. `conflict` сохраняется только как специальный термин несовместимости;
3. активный `CDM` почти не нуждается в дополнительной чистке;
4. основной semantic debt по слову `conflict` уже находится не в теории, а в
   legacy-переходниках и некоторых applied `MMCF`-хвостах.
