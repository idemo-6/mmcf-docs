# CDM/MMCF relative to Scrum, Kanban, OODA, and similar approaches

Language: EN
Status: working draft
Scope: `mmcf-docs`
Updated: 2026-03-13

## Purpose

This document is a short entry-positioning note:

1. what `CDM/MMCF` is relative to familiar workflow approaches;
2. why `CDM/MMCF` should not be framed as just another competing
   "task management methodology";
3. how `CDM/MMCF` relates to `Scrum`, `Kanban`, `OODA`, `Agile`, `Lean`,
   and `Linear`.

For deeper positioning and adoption logic, see:

- [POSITIONING_AND_ADOPTION_ROADMAP](./positioning/POSITIONING_AND_ADOPTION_ROADMAP.md)
- [MMCF_COMPATIBILITY_AND_ADOPTION_MODES](./positioning/MMCF_COMPATIBILITY_AND_ADOPTION_MODES.md)
- [MMCF_LINEAR_COMPATIBILITY_CASE](./cases/MMCF_LINEAR_COMPATIBILITY_CASE.md)
- [MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE](./cases/MMCF_SELF_HOSTING_AND_SELF_REFERENCE_CASE.md)

## Short thesis

`CDM/MMCF` does not replace `Scrum`, `Kanban`, `OODA`, or similar familiar
approaches.

It provides a deeper layer for:

- change semantics;
- work-unit boundaries;
- phase discipline;
- transition and gateway semantics;
- applicability and result;
- governance, traceability, and closure.

Short formula:

- `CDM` answers what change is;
- `MMCF` answers how change is governed;
- familiar methods answer in which working mode people do that;
- tool layers such as `Linear` answer where this is operationally materialized.

## Model layers

### 1. `CDM`

`CDM` is the canonical foundation.

It defines invariants such as:

- `LifeCycle`;
- `ChangeFlow`;
- `PhaseTransition`;
- `Applicable`;
- `Result`;
- `Context`;
- `Experience`.

This is not a process trick or a set of management rituals.
It is a model of change itself.

### 2. `MMCF`

`MMCF` is the applied management and governance layer on top of `CDM`.

It adds:

- the pre-flow layer `Delta / DeltaRecord / DeltaRegistry / PromotionDecision`;
- operational phase discipline;
- explicit gateway and authority logic;
- planning, versioning, and claim governance;
- delivery bridges into real tools and execution contours.

### 3. Workflow approaches

`Scrum`, `Kanban`, `OODA`, `Agile-like`, and similar approaches are better
understood here as external operating profiles.

They may define:

- cadence;
- familiar user-facing vocabulary;
- rituals and organizational expectations;
- visualization and prioritization modes.

But they do not have to be the deepest semantic layer.

## Relation to system dynamics

Since change is part of system dynamics, `CDM/MMCF` can be treated as a
framework for managing system dynamics in the specific part where a system
changes through observed and executed intervention flows.

But there is an important boundary.

`CDM/MMCF` is strongest in:

- structuring change;
- governing change contours;
- separating observed delta from materialized work;
- governance and closure;
- traceable human and agent execution.

Classical `system dynamics` is strongest in:

- feedback loops;
- stocks and flows;
- delays;
- quantitative simulation;
- emergent nonlinear behavior.

So `CDM/MMCF` does not replace classical `system dynamics`.
It is better at describing and disciplining the governed execution of changes
in a system than at simulating the system itself.

## Relative to Agile

`Agile` provides a value frame:

- adaptability;
- fast feedback;
- incremental delivery;
- prioritizing real outcomes over bureaucracy.

But `Agile` is intentionally not a strict formal model of change semantics.

`CDM/MMCF` complements `Agile` where stronger structure is needed:

- clearer work-unit boundaries;
- formal applicability and result discipline;
- stricter closure semantics;
- auditable decision and transition traces.

So the correct formula is:

`Agile` is not replaced by `CDM/MMCF`;  
`CDM/MMCF` gives `Agile` a stricter semantic and governance layer.

## Relative to Scrum

`Scrum` is strong as a team operating profile.

It is good at defining:

- roles;
- cadence;
- incremental organization of work;
- regular synchronization points;
- a clear team rhythm.

But `Scrum` usually does not define a strict enough model for:

- the difference between raw observed problems and materialized work;
- atomic `ChangeFlow`;
- separate `PhaseTransition` semantics;
- applicability and result;
- terminal closure and repeat semantics.

So `Scrum` can be understood as a team mode on top of `MMCF`.

Formula:

- not `MMCF versus Scrum`;
- but `Scrum` as a compatible operating profile on top of `CDM/MMCF`.

## Relative to Kanban

`Kanban` is strong as a flow-management approach.

It is especially good at:

- pull logic;
- WIP discipline;
- flow visualization;
- queue policy;
- local throughput optimization.

But by itself `Kanban` usually gives weaker formalization of:

- what exactly moves through the flow;
- where there is still only `Delta` and where there is already a materialized
  work unit;
- where a bottleneck belongs to phase work and where it belongs to `PT/gateway`;
- when a new flow is required instead of continuing the old one;
- what operational closure exactly means.

So `Kanban` fits well as a flow profile on top of `DeltaRegistry`,
`ChangeFlow`, and `PT` logic in `MMCF`.

## Relative to OODA

`OODA` is strong as a compact decision-loop model:

- `Observe`
- `Orient`
- `Decide`
- `Act`

In that sense, `OODA` is very useful as a high-level phase intuition.

But for governed operational execution of complex changes it is often too
compressed.

It often lacks explicit distinction between:

- the pre-flow delta layer;
- `forecast` and `decide`;
- `implement` and `evaluate`;
- phase state and transition state;
- terminal exits such as `done/repeat/final/delayed`.

So `OODA` is better understood not as a competitor, but as a possible
compressed phase projection on top of the more detailed `CF-6` logic.

## Relative to Lean

`Lean` is strong as a framework for reducing waste, improving flow, and
increasing value.

But `Lean` does not aim to be a strict ontology of change units and gateway
logic.

`CDM/MMCF` can strengthen `Lean` where it is useful to:

- formally separate observation from execution;
- preserve an atomic work unit;
- make evaluate and operational result explicit;
- distinguish correction, retry, repeat, and terminal closure.

## Why this fits Linear well

The `Linear` case matters because `MMCF` fit a real agile-like system mostly
through technical mapping rather than semantic deformation of the model.

This is visible in mappings such as:

- `Backlog` as upstream `DeltaRegistry`;
- `Todo` as the current visible pre-start materialized `ChangeFlow`;
- `In Progress` as active `ChangeFlow`;
- comments, child issues, labels, and fields as carriers of phase, PT, and
  evaluate trace.

This matters because it shows:

1. `MMCF` does not require a brand new platform first;
2. `MMCF` can sit underneath an existing agile/tool workflow;
3. the team's familiar mode can stay intact while semantic depth and governance
   increase.

That is why the current `Linear` case should be treated not as "MMCF became one
more board methodology", but as proof that a deeper change/governance layer is
compatible with a real operational tool contour.

## Practical adoption formula

If this needs to be explained to a team without methodological overload, the
formula should be:

1. there is no need to abandon familiar `Scrum/Kanban/Agile/OODA` language;
2. there is no need to rebuild all rituals first;
3. `CDM/MMCF` can be introduced as a deeper layer of semantics, traceability,
   and governance;
4. start with compatibility mode, then augmentation, then governed mode.

This matches the project's current positioning:

- not replacement-first;
- but compatibility-first;
- then augmentation;
- then governed/native usage.

## Shortest formula

If this needs to be explained very briefly:

`Scrum`, `Kanban`, `OODA`, and similar approaches define the working mode.  
`CDM/MMCF` defines the model of what that mode is actually governing.

Or even shorter:

`CDM/MMCF` is not just another task methodology.  
It is a deeper layer of change semantics, governance, and traceability.
