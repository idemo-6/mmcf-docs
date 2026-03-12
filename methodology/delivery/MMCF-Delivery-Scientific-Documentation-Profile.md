---
title: "MMCF Delivery: профиль научной документации"
date: 2026-03-12
tags: [MMCF, delivery, scientific-documentation, claims, profile]
status: profile-draft
---

# MMCF Delivery: профиль научной документации

## 1. Область

Этот профиль определяет интерпретацию terminal `ChangeFlow` issues
для научной, теоретической и насыщенной методологией документации.

Он применяется к terminal issues с `Work Domain=artifact` и
`Artifact Type=scientific-documentation`.

Профиль начинается только после того, как соответствующая `Delta` уже прошла
promotion из `DeltaRegistry` в terminal `ChangeFlow`.

Raw `DeltaRecord` остается upstream-слоем и не является scientific-documentation terminal issue.

Типовые цели:

1. канонические черновики;
2. теоретические профили;
3. отчеты о валидации и экспериментах;
4. методологические матрицы выравнивания;
5. исследовательские заметки со структурой claims.

Этот профиль расширяет общий профиль документации следующими слоями:

1. `ClaimStatus`;
2. дисциплиной работы с evidence;
3. проверками совместимости с `canon`;
4. более жесткие требования к `evaluate`.

Режим применимости claims для этого профиля: `claim-required`, как определено в:

- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)

---

## 2. Носитель и оси governance

В этом профиле носителем обычно является:

1. теоретический документ;
2. методологический или профильный документ;
3. пакет валидации;
4. набор документации со структурой claims.

Две оси governance должны оставаться раздельными:

1. `EntityVersion` = процессное состояние документа-носителя;
2. `ClaimStatus` = зрелость конкретных claims.

Terminal issue отслеживает один `ChangeFlow`.
Он не должен кодировать `ClaimStatus` в статусе задачи.

---

## 3. Типовые классы задач

Типовые классы терминальных задач:

1. формализовать claim или инвариант;
2. переписать раздел для канонической совместимости;
3. прикрепить protocol к существующему claim;
4. прикрепить подтверждения валидации;
5. подготовить candidate claim к принятию в `canon`;
6. согласовать конфликтующие формулировки между документами.

---

## 4. Интерпретация фаз

### `collect`

Минимум:

1. определить документ-носитель;
2. определить целевую дельту claim/document;
3. зафиксировать релевантный контекст `canon/profile`.

Типовые подтверждения:

- конфликтующие формулировки;
- отсутствующий protocol;
- устаревший claim status;
- неразрешенное замечание рецензента;
- отсутствующий пакет валидации.

### `analyze`

Минимум:

1. локализовать текущее состояние claim/document;
2. определить управляющие references и ограничения совместимости;
3. классифицировать, является ли целевая дельта смысловой, evidential,
   структурной или относящейся к governance.

### `forecast`

Минимум:

1. предложить допустимые пути переписывания и пополнения evidence;
2. сравнить риск смыслового дрейфа;
3. определить ожидаемое влияние на claim-status.

### `decide`

Минимум:

1. выбрать один путь;
2. подтвердить применимость в текущем контексте `canon/profile`;
3. зафиксировать целевое изменение claim/document для данного прохода.

### `implement`

Интерпретация для научной документации:

1. текст документа, реестр evidence или связанный protocol/report реально
   обновлен;
2. выбранная смысловая структура материально применена;
3. claim/document contour получил реальное зафиксированное изменение.

### `evaluate`

Минимум:

1. определить runtime result;
2. проверить смысловую совместимость с управляющими документами;
3. проверить достаточность evidence для целевого шага;
4. зафиксировать, есть ли последствия для `ClaimStatus`.

---

## 5. Требования к `evaluate`

Рекомендуемый список проверок для `evaluate`:

1. совместимость с canon/profile references;
2. явные evidence refs для всех измененных claims в охвате;
3. отсутствие скрытого skip-level move в `ClaimStatus`;
4. отсутствие смешения `EntityVersion` и `ClaimStatus`;
5. явная пометка, предлагается ли обновление claim-status, выполняется ли оно
   в другом месте или не затронуто.

Рекомендуемые дополнительные поля в сводке `CF6` для этого профиля:

1. `claim_ids_affected`
2. `claim_status_impact`
3. `canon_compatibility`
4. `protocol_or_evidence_refs`

---

## 6. Семантика выходов

### `done`

Использовать, когда:

1. целевая дельта по научной документации достигнута;
2. смысловые и governance-проверки пройдены;
3. `Result=+1`;
4. немедленный следующий `CF` не требуется.

### `repeat`

Использовать, когда:

1. текущий проход завершен;
2. следующий немедленный проход уже понятен;
3. следующий поток имеет более специфичную дельту.

Типовые случаи:

1. раздел переписан, но еще не хватает прикрепления протокола;
2. protocol прикреплен, но подтверждения валидации все еще требуют собственного прохода;
3. выравнивание исправлено в одном документе, но связанные документы все еще требуют синхронизации.

### `final`

Использовать, когда:

1. линия изменения научной документации закрывается без планируемого
   продолжения;
2. либо запрошенное изменение неприменимо в текущем контексте `canon/profile`
   (`Result=0`);
3. либо `Result=-1`, и negative outcome policy/authority verdict выбрали
   terminal stop.

### `delayed`

Использовать, когда:

1. немедленное corrective продолжение не запускается;
2. либо изменение сейчас неприменимо (`Result=0`);
3. либо `Result=-1`, и возврат зависит от будущих evidence, protocol, правил
   или решения authority/reviewer.

Типовые условия возврата:

1. завершение эксперимента;
2. финализация protocol;
3. решение реестра claims;
4. обновление `canon` выше по контуру;
5. разрешение междокументного конфликта.

---

## 7. Предметные заметки

1. Слияние документа не равняется валидации claims.
2. Успешная правка текста все равно может требовать `repeat`, если evidential-слой неполон.
3. Обновления `ClaimStatus` должны быть трассируемы, но остаются отдельной осью относительно статуса терминальной задачи.
4. `Result=0` означает неприменимость, а не "недостаточно доказано".

---

## 8. Ссылки

- [MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile](../MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md)
- [MMCF-Delivery-Documentation-Profile](./MMCF-Delivery-Documentation-Profile.md)
- [MMCF-Delivery-Linear-Profile](./MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](./MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Linear-Versioning-Profile](./MMCF-Delivery-Linear-Versioning-Profile.md)
- [MMCF-Delivery-Claim-Maturity-Applicability-Profile](./MMCF-Delivery-Claim-Maturity-Applicability-Profile.md)
- [Claim-Maturity-Canonical](../Claim-Maturity-Canonical.md)
- [Versioning-Canonical](../Versioning-Canonical.md)
- [MMCF-CDM-Alignment-Matrix](../MMCF-CDM-Alignment-Matrix.md)
- [CDM ChangeFlow-6](../../../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CDM Experience](../../../fcdm-core/theory/cdm/Specifications/Experience/Experience-Canonical.md)
