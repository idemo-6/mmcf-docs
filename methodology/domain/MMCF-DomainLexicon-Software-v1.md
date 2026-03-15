---
title: "MMCF DomainLexicon: Software Pilot v1"
date: 2026-03-05
tags: [MMCF, CDM, FROR, DomainLexicon, software, pilot]
status: profile-draft
---

# MMCF DomainLexicon: Software Pilot v1

## 1. Назначение

Документ задает доменный лексикон для первого пилота MMCF в программной
разработке.

Цель:

1. в командной работе использовать понятные доменные термины;
2. сохранять каноническую трассировку к CDM/FROR;
3. не смешивать операционный (командный) язык и онтологический канон.

Общий installable-профиль доменных пакетов, частью которого этот документ
должен стать далее, определен в:

- [MMCF-Domain-Pack-Profile](./MMCF-Domain-Pack-Profile.md)

---

## 2. Принцип двух слоев

### 2.1 External (team-facing) layer

Во внешних документах пилота допускается domain-first язык.

### 2.2 Internal (traceability) layer

Во внутренних артефактах качества сохраняются канонические индексы:

- `F1..F6`
- `CF1..CF6`
- `Result in {+1,0,-1}`
- `Applicable(Intent, C_active, LC_phase)`

Это обязательное условие совместимости с CDM/FROR.

---

## 3. LifeCycle mapping (Software)

Принятые доменные имена для LC-фаз:

1. `F1 -> initialization`
2. `F2 -> develop`
3. `F3 -> exploitation`
4. `F4 -> degrade`
5. `F5 -> turn`
6. `F6 -> end`

Примечание:

- если в команде используется написание `develope/expluatation`, оно
  фиксируется как legacy-алиас, но рабочими именами считаются
  `develop/exploitation`.

---

## 4. ChangeFlow mapping (Software)

Каноническая шестерка сохраняется полностью:

1. `CF1 -> collect`
2. `CF2 -> analyze`
3. `CF3 -> forecast`
4. `CF4 -> decide`
5. `CF5 -> implement`
6. `CF6 -> evaluate`

Важно:

- `decide` обязателен как отдельная фаза;
- `evaluate` не сливается с `implement` в отчетности качества.

---

## 5. Result and Applicability mapping

Доменная интерпретация результата:

1. `+1`: изменение применимо и дало ожидаемый полезный эффект.
2. `0`: изменение неприменимо в текущем контексте/фазе (`ApplicabilityFailure`).
3. `-1`: изменение применимо, но дало деградационный/неуспешный эффект.

Норматив:

`Result=0 <=> ApplicabilityFailure`.

---

## 6. Gateway mapping

Для пилота вводятся team-facing термины:

1. `handoff` — переход `CFi => CF(i+1)`;
2. `handoff failure` — отказ gateway;
3. `handoff owner` — владелец перехода.

Минимум логирования для каждого handoff:

- `from_phase`, `to_phase`, `result`, `reason`, `timestamp`.

---

## 7. FROR alignment in software terms

Доменные соответствия для time/cost слоя:

1. `tau` -> accumulated fixation cost
2. `c_fix` -> commit/write fixation cost
3. `c_bg` -> background run cost
4. `D` -> accumulated background dissipation

Правило:

- обсуждение времени/стоимости в пилоте ведется доменными словами, но
  метрики рассчитываются по FROR-совместимой схеме.

---

## 8. Forbidden terms in pilot docs

В новых командных документах пилота запрещены:

1. `ODS`
2. `OmniCycle`
3. `forcast`
4. `collectData`
5. смешение `Result=0` с no-cost transition

---

## 9. YAML profile (machine-readable)

```yaml
domain: "software_engineering"
mode: "domain-first-with-canonical-trace"

lifecycle:
  F1: { term: "initialization", legacy_aliases: [] }
  F2: { term: "develop", legacy_aliases: ["develope"] }
  F3: { term: "exploitation", legacy_aliases: ["expluatation"] }
  F4: { term: "degrade", legacy_aliases: [] }
  F5: { term: "turn", legacy_aliases: [] }
  F6: { term: "end", legacy_aliases: [] }

changeflow:
  CF1: { term: "collect" }
  CF2: { term: "analyze" }
  CF3: { term: "forecast" }
  CF4: { term: "decide" }
  CF5: { term: "implement" }
  CF6: { term: "evaluate" }

runtime:
  result_zero_only_for: "ApplicabilityFailure"
  require_cf4_decide: true
  require_cf6_evaluate: true

gateway:
  term: "handoff"
  required_fields: ["from_phase", "to_phase", "result", "reason", "timestamp"]

fror_mapping:
  tau: "accumulated_fixation_cost"
  c_fix: "commit_write_fixation_cost"
  c_bg: "background_run_cost"
  D: "accumulated_background_dissipation"
```

---

## 10. Normative references

- [MMCF-Term-Legacy-Mapping](../governance/MMCF-Term-Legacy-Mapping.md)
- [MMCF-Conflict-and-Applicability-Profile](../context/MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Operational-Roles-and-Gateways](../operational/MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Time-and-Cost-FROR-Alignment](../governance/MMCF-Time-and-Cost-FROR-Alignment.md)
- [CDM DomainLexicon](../../../fcdm-core/theory/cdm/DomainLexicon.md)
- [FROR DomainLexicon](../../../fcdm-core/theory/fror/FROR_DomainLexicon.md)
