# MMCF: Linear Compatibility Case

Дата: 12 марта 2026
Статус: рабочий черновик
Язык: RU

## Назначение

Этот документ фиксирует первый доказательный compatibility-case для `CDM/MMCF`:

- наложение `MMCF delivery` на `Linear`.

Цель документа:

1. показать, что `MMCF` уже продемонстрировал рабочую совместимость с реальным
   инструментальным контуром;
2. отделить техническую адаптацию к tool-layer от смысловой деформации модели;
3. зафиксировать, что первый кейс подтверждает именно tool/operational
   compatibility, а не завершенность всей методологии.

Связанные документы:

- [POSITIONING_AND_ADOPTION_ROADMAP](../positioning/POSITIONING_AND_ADOPTION_ROADMAP.md)
- [MMCF_COMPATIBILITY_AND_ADOPTION_MODES](../positioning/MMCF_COMPATIBILITY_AND_ADOPTION_MODES.md)
- [MMCF-Domain-Pack-Profile](../methodology/domain/MMCF-Domain-Pack-Profile.md)
- [MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE](./MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE.md)
- [MMCF-Delivery-Linear-Profile](../methodology/delivery/MMCF-Delivery-Linear-Profile.md)

---

## 1. Основной тезис

Кейс `Linear` важен не потому, что `Linear` является привилегированным
инструментом, а потому, что он стал первым реальным operational contour,
на котором описанная часть `MMCF` легла без существенной смысловой деформации.

То есть:

- это было в первую очередь техническое внедрение;
- это не было подгонкой канона под ограничения чужой методологии;
- это не было переписыванием `MMCF` под backlog-jargon.

Именно поэтому `Linear` можно считать первым доказательным compatibility-case.

---

## 2. Что именно было проверено

В `Linear` была практически разложена уже существующая часть `MMCF`:

1. `Project` как parent lifecycle contour;
2. `Epic` как child lifecycle contour;
3. terminal issue как один `ChangeFlow`;
4. phase discipline `collect -> analyze -> forecast -> decide -> implement -> evaluate`;
5. обязательный `CF6`;
6. terminal exits `done / repeat / final / delayed`;
7. отдельный `PT/gateway` layer;
8. planning contour;
9. version window;
10. claim-aware closure;
11. skills и governance pipelines вокруг этого слоя.

Это не было просто “рисованием новых статусов”.
Это было проверкой того, выдерживает ли модель перенос в реальную tool-систему.

---

## 3. Что подтвердилось

### 3.1 Tool compatibility

Подтвердилось, что `MMCF` можно проецировать в issue-tracker и сохранять:

1. `one terminal issue = one ChangeFlow`;
2. обязательность `evaluate`;
3. отдельность phase и `PhaseTransition`;
4. separate runtime result и operational closure;
5. entity-centric versioning как snapshot/window;
6. claim-governance как отдельную ось.

### 3.2 Operational viability

Подтвердилось, что уже описанная часть `MMCF`:

1. операционно исполнима;
2. пригодна для разработки ПО;
3. пригодна для документации и scientific documentation;
4. достаточно формальна, чтобы на неё опирать skills и repo-level governance.

### 3.3 Compatibility-first positioning

Подтвердилось, что `MMCF` можно внедрять не как replacement workflow,
а как deeper layer под существующим инструментальным режимом.

Это усиливает общую adoption strategy:

- сначала compatibility;
- затем augmentation;
- затем governed mode.

---

## 4. Что особенно важно в этом кейсе

Главное наблюдение:

в `Linear` пришлось делать в основном **техническую адаптацию**, а не
**смысловую деформацию**.

### 4.1 Что пришлось адаптировать технически

1. naming статусов;
2. type mapping в `Linear`;
3. раскладку по custom fields / body / comments;
4. templates;
5. labels как issue-level visibility layer;
6. skills и MCP workflow;
7. version sync ritual;
8. governance pipelines вокруг repo-layer.

Это нормальная адаптация к инструменту.

### 4.2 Чего не пришлось ломать

Не пришлось:

1. отказываться от `CF1..CF6`;
2. убирать `evaluate`;
3. смешивать `Result` и status;
4. уничтожать `PT` как отдельный слой;
5. отказываться от version window;
6. кодировать claims внутри version;
7. подменять `ApplicabilityFailure` backlog-эвфемизмами.

Это и есть ключевой аргумент в пользу зрелости уже описанной части `MMCF`.

---

## 5. Что этот кейс доказывает

Кейс `Linear` уже позволяет делать предварительные выводы.

Он доказывает:

1. операционную состоятельность уже описанной части `CDM/MMCF`;
2. её совместимость с реальной tool-system;
3. её пригодность как более глубокого change/governance layer;
4. возможность внедрять `MMCF` поверх привычного workflow без обязательной
   ранней смены парадигмы пользователя.

---

## 6. Чего этот кейс пока не доказывает

`Linear`-кейс пока не доказывает:

1. завершенность ролевой модели `MMCF`;
2. завершенность authority-layer;
3. универсальность для всех доменов;
4. зрелость будущей нативной runtime/system реализации;
5. полную совместимость `MMCF` со всеми внешними методологиями как целыми
   social/process frameworks.

То есть это сильный, но ограниченный по уровню абстракции кейс.

Корректная формула:

- `Linear` доказывает tool/operational compatibility;
- он не доказывает завершенность всей методологии как полного social-role framework.

---

## 7. Почему это важно для позиционирования

Из этого кейса следует сильная, но аккуратная позиция:

1. `MMCF` не является еще одной issue-tracker methodology;
2. `MMCF` уже показал, что может быть deeper invariant layer под реальным
   delivery-инструментом;
3. `MMCF` не требует сначала заменить существующую практику;
4. `MMCF` может внедряться как augmentation layer;
5. `Linear` на текущем этапе — bootstrap-adapter, а не конечная форма системы.

Это хорошо согласуется с принципом:

- не конкурировать с `Scrum/Agile/Kanban`;
- а усиливать и структурировать change-practice поверх них.

---

## 8. Связь с Compatibility / Adoption Modes

В терминах [MMCF_COMPATIBILITY_AND_ADOPTION_MODES](../positioning/MMCF_COMPATIBILITY_AND_ADOPTION_MODES.md)
кейс `Linear` показывает:

1. `Compatibility mode` уже реален;
2. `Augmented mode` уже реален;
3. элементы `Governed mode` уже частично проступают через versioning,
   planning, claim-governance и pipelines.

То есть это не только proof-of-concept, но и первый рабочий маршрут внедрения.

---

## 9. Связь с Domain Pack

Кейс `Linear` также подтверждает важность [MMCF-Domain-Pack-Profile](../methodology/domain/MMCF-Domain-Pack-Profile.md).

Почему:

1. даже успешная техническая compatibility не отменяет высокую когнитивную
   цену входа;
2. чтобы не вываливать каноническую сложность на пользователя, нужен
   domain-facing слой;
3. значит следующий шаг после tool compatibility — не только новые инструменты,
   но и более качественный пользовательский вход через domain packs.

Иначе система будет сильной под капотом, но слишком тяжелой на входе.

---

## 10. Следующий естественный compatibility-case

После `Linear` следующим логичным кейсом выглядит:

- `OODA`

Но уже на другом уровне:

- не tool compatibility;
- а phase-model compatibility.

То есть `Linear` и `OODA` не конкурируют как кейсы.
Они проверяют разные уровни совместимости.

Отдельно от этого tool-case следующий уже достигнутый слой зафиксирован в:

- [MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE](./MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE.md)

---

## 11. Короткий итог

`Linear` показал, что уже описанная часть `MMCF` легла в реальный
операционный инструмент в основном через техническое отображение, а не через
смысловую подгонку.

Это один из самых сильных аргументов текущего этапа:

- база `CDM/MMCF` уже не выглядит чисто теоретической;
- она уже показала первую рабочую инструментальную совместимость;
- и именно поэтому теперь можно двигаться дальше к новым compatibility-cases,
  domain packs и будущим native profiles.
