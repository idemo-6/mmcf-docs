---
title: "MMCF Delivery: профиль применимости Claim Maturity"
date: 2026-03-12
tags: [MMCF, delivery, claim-maturity, profile, documentation]
status: profile-draft
---

# MMCF Delivery: профиль применимости Claim Maturity

## 1. Область

Этот профиль определяет, когда `Claim Maturity` обязателен, опционален или не
нужен для документов раздела delivery и связанных с ними рабочих объектов.

Профиль нужен, чтобы избежать двух противоположных ошибок:

1. потери управления claims там, где оно методологически обязательно;
2. перегрузки обычной операционной документации лишней бюрократией вокруг claims.

Общий MMCF-профиль применимости claim-governance вынесен в:

- [MMCF-Claim-Governance-Applicability-Profile](../governance/MMCF-Claim-Governance-Applicability-Profile.md)

---

## 2. Базовый принцип

`Claim Maturity` применяется к claims, а не к документам целиком.

При этом классы документов различаются по тому, является ли структура,
несущая claims:

1. обязательной;
2. опциональной;
3. обычно отсутствующей.

Поэтому этот профиль использует три режима применимости:

1. `claim-required`
2. `claim-optional`
3. `claim-none`

---

## 3. Режимы применимости

### 3.1 `claim-required`

Используйте этот режим, когда документ содержательно включает:

1. формальные claims;
2. инварианты;
3. утверждения о методологической совместимости;
4. валидационные вердикты;
5. нормативные высказывания, прилегающие к `canon` или теории.

В этом режиме затронутые claims должны быть трассируемы через:

1. `ClaimId`
2. `ClaimStatus`
3. `EvidenceRefs`
4. явную `evaluate`-заметку о влиянии на claim

### 3.2 `claim-optional`

Используйте этот режим, когда документ не является в первую очередь
реестром claims, но может содержать ограниченный набор сильных утверждений,
которым полезна claim-дисциплина.

Типовые примеры:

1. архитектурные гарантии;
2. гарантии совместимости;
3. предпосылки безопасности;
4. утверждения о производительности;
5. миграционные гарантии.

В этом режиме отслеживания требуют только явно размеченные claims.

### 3.3 `claim-none`

Используйте этот режим, когда документ в первую очередь операционный,
объяснительный, редакционный или навигационный и не выигрывает от
управления claims.

Типовые примеры:

1. инструкции;
2. регламенты;
3. примеры;
4. навигационные страницы;
5. общая продуктовая документация.

В этом режиме `ClaimStatus` не обязателен при закрытии терминальной задачи.

---

## 4. Рекомендуемая классификация по классу документа

| Класс документа | Claim mode | Правило |
|---|---|---|
| научная и теоретическая документация | `claim-required` | по умолчанию |
| канонические документы | `claim-required` | по умолчанию для ключевых claims в области документа |
| матрицы выравнивания | `claim-required` | по умолчанию для нормативных отображений и инвариантов |
| отчеты и протоколы валидации | `claim-required` | по умолчанию |
| архитектурные и спецификационные документы с гарантиями | `claim-optional` | использовать только для явных сильных утверждений |
| общая инженерная документация | `claim-none` | по умолчанию |
| продуктовая и пользовательская документация | `claim-none` | по умолчанию |
| примеры, индексы, глоссарии | `claim-none` | по умолчанию |

---

## 5. Влияние на terminal `ChangeFlow`

### 5.1 Для `claim-required`

Сводка `CF6` должна включать:

1. `claim_ids_affected`
2. `claim_status_impact`
3. `protocol_or_evidence_refs`
4. `canon_compatibility`

### 5.2 Для `claim-optional`

Сводка `CF6` включает claim-поля только тогда, когда issue явно затрагивает
отслеживаемый claim.

### 5.3 Для `claim-none`

Сводка `CF6` не требует полей, специфичных для claims.

---

## 6. Отображение на текущие профили delivery

Рекомендуемые значения по умолчанию:

1. `MMCF Delivery: Scientific Documentation Profile` -> `claim-required`
2. `MMCF Delivery: Documentation Profile` -> `claim-none`
3. `MMCF Delivery: Software Profile` -> `claim-none` по умолчанию,
   `claim-optional` для архитектурных, спецификационных, security- и performance-гарантий
4. `MMCF Delivery: Linear Profile` -> `claim-none`
5. `MMCF Delivery: Terminal ChangeFlow Contract` -> `claim-none`

---

## 7. Антипаттерны

Запрещено или нежелательно:

1. трактовать каждое изменение документа как изменение claim-status;
2. кодировать `ClaimStatus` в статусе терминальной задачи;
3. использовать `claim-required` для примеров, регламентов, индексов или обычных руководств;
4. проводить тихие переходы claim-status без следа evidence.

---

## 8. Ссылки

- [MMCF-Claim-Governance-Applicability-Profile](../governance/MMCF-Claim-Governance-Applicability-Profile.md)
- [Claim-Maturity-Canonical](../governance/Claim-Maturity-Canonical.md)
- [Versioning-Canonical](../governance/Versioning-Canonical.md)
- [MMCF-Delivery-Documentation-Profile](./MMCF-Delivery-Documentation-Profile.md)
- [MMCF-Delivery-Scientific-Documentation-Profile](./MMCF-Delivery-Scientific-Documentation-Profile.md)
- [MMCF-Delivery-Software-Profile](./MMCF-Delivery-Software-Profile.md)
