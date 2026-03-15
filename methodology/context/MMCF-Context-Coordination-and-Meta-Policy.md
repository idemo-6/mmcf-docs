---
title: "MMCF: Context Coordination and Meta Policy"
date: 2026-03-05
tags: [MMCF, Context, C_coord, C_meta, policy, governance]
status: profile-draft
---

# MMCF: Context Coordination and Meta Policy

## 1. Scope

Документ задает минимальный операционный профиль:

1. `C_coord` — координация и разрешение межконтекстных конфликтов;
2. `C_meta` — обучение и адаптация policy для людей и LLM-агентов.

В терминах taxonomy этот документ прежде всего работает с:

1. `context-conflict`;
2. `constraint-conflict`;
3. `authority-conflict`;
4. `resource-conflict`.

---

## 2. Core principle

`C_coord` строится как иерархическая цепочка решений, но не только по орг-структуре.

Нормативно:

1. базовый путь — по иерархии владения;
2. допускается capability-override по risk/evidence/verification;
3. любой override обязан быть аудитируемым.

---

## 3. Priority classes

### P1. Delta intake priority

Приоритет intake и квалификации `Delta` как источника pre-flow линии
изменения.

Требования:

1. реестр `DeltaRecord` или qualified `Delta` по каждому контексту/контуру;
2. классификация: `critical/high/medium/low`;
3. ручная и автоматическая переоценка при изменении условий.

### P2. Executor selection priority

Приоритет выбора исполнителя.

Допускается выбор на основе:

1. опыта;
2. субъективности/пригодности инструментария;
3. креативности (где релевантно);
4. исторической надежности выполнения.

### P3. Decision-center priority

Приоритет центра разрешения `context/constraint/authority/resource-conflict`
реализации.

Требования:

1. назначить `primary authority agent`;
2. назначить `fallback authority agent`;
3. зафиксировать условия переключения primary -> fallback.

---

## 4. Additional mandatory priorities

### P4. Risk and regulatory priority

1. hard-stop ограничения безопасности/комплаенса;
2. запрет обхода hard-stop бизнес-срочностью без formal override.

### P5. Blast-radius priority

Приоритизация по масштабу потенциального ущерба:

1. local
2. cross-team
3. system-critical

### P6. Confidence and evidence priority

Приоритизация по качеству доказательств:

1. наблюдаемые логи/метрики;
2. тесты/симуляции;
3. историческая воспроизводимость.

Низкая уверенность -> эскалация в `C_meta`.

### P7. Cost-of-delay vs cost-of-change priority

Фиксируется баланс:

1. стоимость задержки решения;
2. стоимость ошибочного/деградационного изменения.

### P8. Dependency critical-path priority

Блокирующие зависимости на критическом пути получают повышенный приоритет.

### P9. Authority mode priority

Для каждого класса решений задается режим:

1. `auto`
2. `hybrid`
3. `manual`

---

## 5. Escalation policy (minimum)

Обязательные поля:

1. `escalation_timeout`
2. `escalation_levels[]`
3. `entry_conditions`
4. `exit_conditions`

Правило:

- если межконтекстный, authority- или constraint-конфликт не разрешен в
  timeout текущего уровня, эскалация обязательна.

---

## 6. Override policy

Обязательные поля override-события:

1. `override_reason`
2. `approved_by`
3. `approved_at`
4. `scope`
5. `expires_at`
6. `revalidation_plan`
7. `conflict_class`
8. `conflict_parties`
9. `precedence_basis`

Override не отменяет аудит и последующую переоценку решения.

### 6.1 Authority-conflict protocol

Для `authority-conflict` policy минимум должен задавать:

1. кто может инициировать protocol;
2. какие действия freeze'ятся до resolution;
3. какой precedence basis применяется;
4. кто имеет final fallback authority;
5. когда конфликт считается `blocked`, а когда допускается override.

Минимальные обязательные шаги:

1. зафиксировать `conflict_class=authority-conflict`;
2. зафиксировать `conflict_parties[]`;
3. указать `affected_scope`;
4. применить scoped precedence;
5. если inline resolution не достигнут, перейти на escalation path;
6. после resolution выполнить revalidation affected scope.

---

## 7. Revalidation policy

Переоценка решения обязательна, если:

1. изменился активный контекст или его ограничения;
2. изменился риск-класс;
3. появились новые данные, снижающие confidence;
4. произошло `Result=0` или повторная компенсация.

---

## 8. C_meta learning hooks

`C_meta` должен получать сигналы обучения минимум из:

1. `ApplicabilityFailure` (`Result=0`);
2. gateway failures;
3. повторных компенсаций/rework;
4. инцидентов и near-miss;
5. стабильных success-паттернов (`+1` с высокой воспроизводимостью).

Результат обучения:

1. корректировка приоритетов;
2. корректировка authority mode;
3. корректировка escalation thresholds.

---

## 9. Minimal machine-readable skeleton

```yaml
policy_id: "ctx_coord_meta_v1"

c_coord:
  hierarchy_path: ["team_lead", "domain_lead", "program_board"]
  capability_override: true
  escalation_timeout: "24h"
  escalation_levels: ["L1", "L2", "L3"]

priorities:
  delta_intake: { enabled: true, registry_required: true }
  executor_selection: { enabled: true }
  decision_center: { enabled: true, primary_fallback_required: true }
  risk_regulatory: { enabled: true, hard_stop: true }
  blast_radius: { enabled: true }
  confidence_evidence: { enabled: true }
  cod_vs_coc: { enabled: true }
  dependency_critical_path: { enabled: true }
  authority_mode: { enabled: true, modes: ["auto", "hybrid", "manual"] }

override:
  required_fields:
    ["override_reason", "approved_by", "approved_at", "scope", "expires_at", "revalidation_plan", "conflict_class", "conflict_parties", "precedence_basis"]

revalidation:
  triggers: ["context_changed", "risk_changed", "confidence_drop", "result_zero", "rework_repeat", "authority_conflict"]

c_meta:
  learning_hooks: ["result_zero", "gateway_failure", "rework", "incident", "stable_success"]
```

---

## 10. Acceptance checklist

1. Есть формальная цепочка `C_coord` и escalation timeouts.
2. Есть primary/fallback authority для квалификации `Delta` и конфликтных решений.
3. Есть hard-stop риск/регуляторный слой.
4. Есть формальный override с аудитом и revalidation.
5. Есть отдельный protocol для `authority-conflict`.
6. Есть learning hooks и обновление policy через `C_meta`.

---

## 11. References

- [MMCF-Context-Canonical](./MMCF-Context-Canonical.md)
- [MMCF-Context-Examples-Sales-CRM](./MMCF-Context-Examples-Sales-CRM.md)
- [MMCF-Context-Examples-Product-Platform](./MMCF-Context-Examples-Product-Platform.md)
- [MMCF-Conflict-and-Applicability-Profile](./MMCF-Conflict-and-Applicability-Profile.md)
- [MMCF-Conflict-Taxonomy-Canonical](./MMCF-Conflict-Taxonomy-Canonical.md)
- [Post-Anthropocentric Computing Manifesto](../../../idemo-docs/vision/Post-Anthropocentric_Computing_Manifesto.md)
