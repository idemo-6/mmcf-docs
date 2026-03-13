# Ревизия MMCF / CDM / FROR и `mmcf-code`

Дата: 12 марта 2026

## 1. Область проверки

Проверка охватывала четыре вопроса:

1. перспективность и жизнеспособность самой концепции `CDM/MMCF`;
2. логическую согласованность `MMCF` с `CDM` и `FROR`;
3. внутреннюю согласованность ядра `MMCF` и слоя `delivery/linear`;
4. полноту задокументированности `mmcf-code`.

Основной корпус проверки:

- [MMCF-Canonical](../methodology/MMCF-Canonical.md)
- [MMCF-Minimal-Working-Model](../methodology/MMCF-Minimal-Working-Model.md)
- [MMCF-CDM-Alignment-Matrix](../methodology/MMCF-CDM-Alignment-Matrix.md)
- [MMCF-Applicability-Result-Alignment-Matrix](../methodology/MMCF-Applicability-Result-Alignment-Matrix.md)
- [MMCF-Operational-Roles-and-Gateways](../methodology/MMCF-Operational-Roles-and-Gateways.md)
- [MMCF-Operational-Work-Unit-Contract](../methodology/MMCF-Operational-Work-Unit-Contract.md)
- [Versioning-Canonical](../methodology/Versioning-Canonical.md)
- [Claim-Maturity-Canonical](../methodology/Claim-Maturity-Canonical.md)
- [MMCF-Delivery-Linear-Profile](../methodology/delivery/MMCF-Delivery-Linear-Profile.md)
- [MMCF-Delivery-Terminal-ChangeFlow-Contract](../methodology/delivery/MMCF-Delivery-Terminal-ChangeFlow-Contract.md)
- [MMCF-Delivery-Linear-Planning-Profile](../methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md)
- [MMCF-Delivery-Linear-Versioning-Profile](../methodology/delivery/MMCF-Delivery-Linear-Versioning-Profile.md)
- [MMCF-Delivery-Codex-Skills-Profile](../methodology/delivery/MMCF-Delivery-Codex-Skills-Profile.md)
- [mmcf-code/README](../mmcf-code/README.md)
- [CODEX_SKILLS_QUICKSTART](../mmcf-code/docs/CODEX_SKILLS_QUICKSTART.md)
- [LINEAR_MMCF_SETUP](../mmcf-code/docs/LINEAR_MMCF_SETUP.md)
- [GOVERNANCE_PIPELINES_QUICKSTART](../mmcf-code/docs/GOVERNANCE_PIPELINES_QUICKSTART.md)
- [CLAIM_GOVERNANCE_PIPELINE](../mmcf-code/docs/CLAIM_GOVERNANCE_PIPELINE.md)
- [VERSION_GOVERNANCE_PIPELINE](../mmcf-code/docs/VERSION_GOVERNANCE_PIPELINE.md)

Референсная база `CDM/FROR`:

- [ChangeFlow-6_v3](../fcdm-core/theory/cdm/Specifications/ChangeFlow-6_v3.md)
- [CF-LC-Evaluate](../fcdm-core/theory/cdm/Specifications/CF-LC-Evaluate.md)
- [PhaseTransition-CF](../fcdm-core/theory/cdm/Specifications/PhaseTransition_Specifications/PhaseTransition-CF.md)
- [CDM Versioning Canonical](../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md)
- [FROR_CDM_bridge](../fcdm-core/theory/cdm/bridge/FROR_CDM_bridge.md)
- [FROR_claims_git_workflow](../fcdm-core/theory/fror/FROR_claims_git_workflow.md)

## 2. Краткий вывод

Общая оценка: концепция `MMCF` выглядит перспективной и жизнеспособной, если рассматривать ее не как замену обычному task management, а как управленческий слой для сложных, многоконтекстных, claim-bearing и governance-heavy изменений. Сильная сторона подхода в том, что он аккуратно разделяет:

- runtime-результат и operational closure;
- фазу и фазовый переход;
- versioning сущности и maturity claims;
- planning самой задачи и matching исполнителя;
- канон `CDM` и applied-layer `MMCF`.

В текущем состоянии ядро `MMCF` уже достаточно цельное, чтобы служить рабочей методологией для:

- канонических и научных документов;
- межкомандной разработки с жёсткими gateway/approval режимами;
- governance-heavy delivery;
- исследовательских и высоковариативных change-потоков.

Главный риск не в том, что модель нелогична, а в том, что она дорогая по когнитивной и операционной дисциплине. Без автоматизации и без зрелой команды её легко превратить в перегруженный слой “правильных слов” без реального выигрыша в управляемости.

## 3. Перспективность концепции

### 3.1 Сильные стороны

1. `MMCF` правильно позиционируется как applied-layer над `CDM`, а не как конкурирующая онтология. Это видно в [MMCF-Canonical](../methodology/MMCF-Canonical.md) и поддерживается alignment-матрицами.
2. Очень сильное место модели — разведение независимых осей:
   - `Result` vs terminal exit;
   - `Phase` vs `PhaseTransition`;
   - `EntityVersion` vs `ClaimStatus`;
   - task planning vs executor matching.
3. Хорошо сделана интеграция с `FROR`: `Result=0` не смешивается с `FROR.no_cost_transition`, а runtime и analytical layers разведены явно.
4. `delivery/linear` не пытается переписать канон под инструмент, а аккуратно делает tool-profile поверх него.
5. Модель уже достаточно operationalized: есть contract для terminal issue, planning profile, versioning profile, skills и repo-level governance pipelines.

### 3.2 Ограничения и риски

1. Высокая когнитивная цена входа. Чтобы применять модель корректно, команде нужно понимать не только `Linear`, но и `ChangeFlow`, `PhaseTransition`, `Applicability`, `Claim`, `Versioning`, `CreativeChangeFlow`, `MetaChangeFlow`.
2. Высокая цена дисциплины. Модель особенно сильна там, где готовы вести явные contracts, gateway traces, `CF6`, version notes, claim refs и review rituals.
3. Риск “методологического театра”. Если event logs, gateways, version windows и claims описываются только текстом без автоматической проверки, часть ценности теряется.
4. Для обычного low-ceremony продуктового backlog модель избыточна. В этом режиме её стоит применять выборочно, а не как обязательный слой на все задачи подряд.

### 3.3 Где концепция особенно перспективна

- theory / canon / scientific documentation;
- архитектурные и governance-heavy изменения;
- межконтурные изменения с handoff и approval bottlenecks;
- regulated / audit-heavy процессы;
- внедрение смешанных human + AI delivery контуров.

### 3.4 Где применять осторожно

- короткие одношаговые продуктовые задачи;
- массовый мелкий backlog без claim/version governance;
- команды, у которых ещё нет культуры explicit handoff и explicit evaluate.

## 4. Согласованность с `CDM` и `FROR`

В целом согласованность хорошая.

Что уже сделано правильно:

1. `Result=0` жёстко удержан как `ApplicabilityFailure`, а не как “нейтральный исход”.
2. `GatewayFailure` разведён с `ApplicabilityFailure`.
3. retry внутри `CF-PT` описан так, что не ломает каноническое правило: только terminal `pt_result=false` ведёт поток в `CF6`.
4. `ClaimStatus` и `EntityVersion` разведены как независимые governance-оси.
5. `FROR` используется как аналитический слой, а `CDM` — как runtime/ontological layer.

Главный внешний зазор сейчас только один: versioning source-of-truth формально описан в `MMCF` и одновременно инвертирован в одном из `CDM`-документов. Это не ломает всю модель, но создаёт реальное нормативное двусмыслие.

## 5. Внутренняя согласованность ядра `MMCF` и `delivery/linear`

В целом ядро и `delivery/linear` сейчас согласованы лучше, чем на раннем этапе.

Особенно удачные решения:

1. terminal issue = один `ChangeFlow`;
2. `evaluate` оставлен обязательным и не смешан с completed metrics;
3. `done / repeat / final / delayed` разведены как operational exits;
4. `PT`-слой вынесен отдельно от phase status;
5. versioning в `Linear` сделан как snapshot/window сущности, а не как “версия тикета”.

Главные remaining gaps:

1. нет нормального terminal closure для случая `Result=-1`, когда corrective flow не запускается немедленно;
2. в корневом planning-слое ещё сосуществуют две языковые модели:
   - старая: `CI / V`
   - новая: `Flow Mode / Variativity Target / Decide Policy`
   Граница между ними уже намечена, но не доведена до одной явной translation policy.
3. `delivery/linear` по-прежнему плохо покрывает meta-operational задачи самим schema-моделем `Artifact Type`.

## 6. Полнота документации `mmcf-code`

Оценка: документация уже достаточна для controlled internal use, но ещё не полностью достаточна для бесшовной передачи “в чужой инстанс Codex” без живого контекста.

Что хорошо покрыто:

1. есть быстрый вход по skills;
2. есть manual setup для `Linear`;
3. отдельно описано, когда нужен governance pipeline, а когда нет;
4. есть install scripts и portable package;
5. version pipeline и claim pipeline разведены по смыслу.

Чего пока не хватает:

1. не везде закрыта переносимость skill package;
2. не описана sync-дисциплина между public source skills в `mmcf-docs` и private mirror в `mmcf-code`;
3. governance docs не проговаривают dependency `PyYAML`, хотя installer её требует;
4. version pipeline docs не достаточно чётко проговаривают свои ограничения как checker continuity/snapshot layer, а не полного CDM derivation layer.

## 7. Findings

### [P1] Нет корректного terminal closure для `Result=-1`, если corrective flow не запускается сразу

Сейчас `MMCF` хорошо покрывает:

- `+1 -> done/repeat`
- `0 -> repeat/final/delayed`

Но сознательно не покрывает кейс:

- `Applicable=true`, `Result=-1`, и немедленного следующего `repeat` нет.

Это явно зафиксировано в:

- [MMCF-Operational-Work-Unit-Contract:150-154](../methodology/MMCF-Operational-Work-Unit-Contract.md#L150)
- [MMCF-Delivery-Linear-Profile:155-161](../methodology/delivery/MMCF-Delivery-Linear-Profile.md#L155)

Проблема в том, что такой поток зависает в `evaluate` не потому, что логика не знает исход, а потому что operational exit matrix неполна. Для живого delivery это слишком жёсткая рамка: часть отрицательных исходов реально требует terminal stop без немедленного повторного прохода.

Рекомендация:

1. либо ввести отдельный stop-class для `Result=-1`;
2. либо явно описать governance-процедуру перевода `-1` в terminal closure без искусственного переобозначения как неприменимости.

### [P1] В versioning остаётся внешний конфликт source-of-truth между `CDM` и `MMCF`

В `MMCF` versioning описан как applied governance profile поверх `CDM`:

- [MMCF Versioning Governance Profile:10-12](../methodology/Versioning-Canonical.md#L10)

Но в `CDM` versioning file прямо написано обратное:

- [CDM Versioning Canonical:10](../fcdm-core/theory/cdm/Specifications/Versioning/Versioning-Canonical.md#L10)

Это создаёт двусмысленный нормативный порядок:

- либо `CDM -> MMCF`,
- либо `MMCF -> CDM`.

Для концепции, которая во многом держится на точной иерархии канона и applied profile, это серьёзный зазор.

Рекомендация:

1. выровнять преамбулу в `fcdm-core` так, чтобы `CDM` был явным source of truth;
2. оставить в `MMCF` только governance/application mapping.

### [P2] `Version Governance Pipeline v1` пока проверяет continuity snapshot-log, но не полный CDM derivation

Документация подаёт pipeline как derivation-oriented checker:

- [VERSION_GOVERNANCE_PIPELINE:45-65](../mmcf-code/docs/VERSION_GOVERNANCE_PIPELINE.md#L45)

Но фактическая реализация проверяет в первую очередь:

- формат версии;
- согласованность `entity_id`;
- непрерывность `pre_version -> post_version`;
- соответствие последнего `post_version` registry snapshot.

Это видно в:

- [check_version_consistency.py:66-83](../mmcf-code/governance-pipelines/versioning/check_version_consistency.py#L66)
- [check_version_consistency.py:134-199](../mmcf-code/governance-pipelines/versioning/check_version_consistency.py#L134)

Checker не валидирует:

1. phase-gate provenance для `inc/lc/cf/cfp`;
2. связь версии с каноническими `LC.Start / LC.PhaseEnter / CF.Start / CF.PhaseEnter / CF.End`;
3. порядок событий по timestamp.

Это не означает, что pipeline плохой. Но сейчас это именно `snapshot/event continuity checker`, а не полный state-derived CDM replayer. Документацию стоит сделать в этом месте жёстче и честнее.

Рекомендация:

1. явно назвать v1 “continuity checker”;
2. отдельно отметить, что порядок event log сейчас считается canonical по порядку записей;
3. оставить полный derivation-replay как future v2.

### [P2] Private skill package ещё не полностью переносим

`mmcf-code` уже оформлен как installable package, но один из skills всё ещё жёстко привязан к локальному пути этой машины:

- [linear-version-sync-review/SKILL.md:13-15](../mmcf-code/codex-skills/linear-version-sync-review/SKILL.md#L13)

Это ломает portability для “других инстансов Codex”, если там нет checkout `mmcf-docs` ровно по тому же абсолютному пути.

Рекомендация:

1. либо перевести этот skill на bundled reference рядом с `linear-mmcf`;
2. либо документировать, что этот skill требует локальный `mmcf-docs` checkout;
3. лучше сделать первое.

### [P2] Есть два набора source skills, но нет явной sync-policy между ними

Сейчас source-of-truth для навыков описан так:

- versioned public source в `mmcf-docs/codex-skills/`
- private mirror в `mmcf-code/codex-skills/`

Это явно видно в:

- [MMCF-Delivery-Codex-Skills-Profile:127-138](../methodology/delivery/MMCF-Delivery-Codex-Skills-Profile.md#L127)

Сама по себе схема рабочая, но в документации нет явного ответа на вопросы:

1. какой репозиторий считается первичным источником изменений;
2. как контролируется drift;
3. когда mirror обязан обновляться;
4. как проверяется идентичность public/private skill sets.

Рекомендация:

1. зафиксировать single source of truth;
2. описать sync-step в `mmcf-code/docs/CODEX_SKILLS_QUICKSTART.md` или отдельном `SKILL_SYNC_POLICY.md`;
3. желательно добавить простой diff-check script.

### [P3] В ядре planning всё ещё сосуществуют две модели без явной translation policy

В корневом слое `methodology` всё ещё живут одновременно:

- `CI / V` как planning profile:
  - [MMCF-Creativity-and-Variability-Profile:12-18](../methodology/MMCF-Creativity-and-Variability-Profile.md#L12)
  - [MMCF-Creativity-and-Variability-Profile:95-105](../methodology/MMCF-Creativity-and-Variability-Profile.md#L95)
- и новая boundary/model:
  - [MMCF-Planning-Assignment-Capability-Boundary:46-68](../methodology/MMCF-Planning-Assignment-Capability-Boundary.md#L46)
  - [MMCF-Delivery-Linear-Planning-Profile:68-91](../methodology/delivery/MMCF-Delivery-Linear-Planning-Profile.md#L68)

Сейчас это не прямое логическое противоречие, но translation layer не сформулирован до конца:

- когда использовать `CI` как методологический scalar;
- как он соотносится с `Flow Mode=creative`;
- считается ли `CI` legacy analytical layer или полноценным корневым planning profile.

Рекомендация:

1. либо оформить короткий bridge `CI/V -> FlowMode/Variativity/DecidePolicy`;
2. либо понизить `CI` до analytical appendix и явно сказать, что operational default moved to the new planning model.

### [P3] `Artifact Type` в `delivery/linear` не покрывает meta-operational work

Сейчас minimal schema допускает только:

- `software`
- `documentation`
- `scientific-documentation`

Это зафиксировано в:

- [MMCF-Delivery-Linear-Profile:194-197](../methodology/delivery/MMCF-Delivery-Linear-Profile.md#L194)

Но сама практика уже вышла за эту тройку: настройка workspace, governance rollout, schema migration и другие meta-operational задачи в эту ось ложатся плохо.

Рекомендация:

1. либо расширить `Artifact Type`;
2. либо ввести отдельную orthogonal ось `Work Domain`;
3. в любом случае не оставлять этот gap скрытым.

### [P3] Документация governance pipelines не проговаривает обязательную dependency `PyYAML`

Installer требует `PyYAML`:

- [install_governance_pipelines.sh:194-197](../mmcf-code/scripts/install_governance_pipelines.sh#L194)

Но в пользовательских docs это явно не сказано:

- [GOVERNANCE_PIPELINES_QUICKSTART](../mmcf-code/docs/GOVERNANCE_PIPELINES_QUICKSTART.md)
- [CLAIM_GOVERNANCE_PIPELINE](../mmcf-code/docs/CLAIM_GOVERNANCE_PIPELINE.md)
- [VERSION_GOVERNANCE_PIPELINE](../mmcf-code/docs/VERSION_GOVERNANCE_PIPELINE.md)

Это не методологическая проблема, а gap handoff-документации. Для внутреннего использования некритично, для передачи в новый репозиторий — заметно.

Рекомендация:

1. добавить короткий prerequisite block;
2. указать минимальную команду установки зависимости.

## 8. Общая оценка по четырём осям

| Ось | Оценка | Комментарий |
|---|---|---|
| Перспективность концепции | высокая с оговорками | Сильна для сложных, governance-heavy и claim-bearing контуров; избыточна для low-ceremony backlog. |
| Согласованность с `FROR/CDM` | хорошая | Основная логика согласована; главный внешний конфликт сейчас в source-of-truth по versioning. |
| Внутренняя согласованность ядра и `delivery/linear` | хорошая, но не завершённая | Каркас цельный, но есть gaps по `Result=-1`, planning language split и `Artifact Type`. |
| Полнота документации `mmcf-code` | средняя / достаточная для controlled use | Пользоваться уже можно; для полноценной передачи “в новый инстанс” нужна ещё одна итерация доводки. |

## 9. Рекомендуемый следующий порядок доработок

1. Закрыть `Result=-1` terminal closure gap.
2. Выровнять порядок `CDM -> MMCF` в versioning across repos.
3. Ужесточить формулировку ограничений `Version Governance Pipeline v1`.
4. Убрать absolute-path зависимость из `linear-version-sync-review`.
5. Описать sync-policy между `mmcf-docs/codex-skills` и `mmcf-code/codex-skills`.
6. Принять решение по `Artifact Type` для meta-operational задач.
7. Свести `CI/V` и новый planning contour через короткий bridge-документ.

## 10. Итог

`MMCF` уже выглядит как сильный applied-layer над `CDM`, а не как набор разрозненных заметок. Самое ценное в текущем состоянии — не отдельные профили, а то, что модель начинает держать границы:

- канон vs профиль;
- runtime vs governance;
- version vs claim;
- phase vs transition;
- delivery-tool vs methodology.

Если закрыть перечисленные зазоры, особенно `Result=-1`, versioning source-of-truth и portability/documentation gaps в `mmcf-code`, связка `CDM + MMCF + delivery/linear + governance pipelines` будет выглядеть уже не как экспериментальная схема, а как реально воспроизводимый рабочий контур.
