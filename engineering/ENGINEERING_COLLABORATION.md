# Engineering Collaboration (MMCF)

Language: EN
Source of Truth: this file
Last Sync: 2026-03-15

## Purpose

Public entry point for methodology contributors working on MMCF operational and governance design.

## Primary Specialist Profile

1. Methodologists (change management, organizational design, governance)
2. Operational framework designers
3. Applied systems analysts

Engineering/architecture participation is welcome as supporting profile for implementation-readiness.

## Current Repository Model

1. `mmcf-docs` is public and contains methodology and operational profiles.
2. `mmcf-code` currently acts primarily as the operational tooling repo for
   MMCF:
   - installable Codex skills
   - governance pipelines
   - setup and sync scripts
3. The methodology layer is currently more mature than the implementation/tool
   layer.

## How External Methodology Contributors Can Contribute Right Now

1. Refine operational role/gateway contracts for `CF1..CF6`.
2. Propose machine-friendly trace/event schemas for phase/gateway telemetry.
3. Improve applicability/failure classification profiles.
4. Contribute implementation-readiness checklists for teams adopting MMCF.
5. Add domain-specific implementation examples consistent with MMCF canon.
6. Help keep methodology claims aligned with the actual scope of `mmcf-code`.

## Technical Entry Points

1. `methodology/core/MMCF-Canonical.md`
2. `methodology/operational/MMCF-Operational-Roles-and-Gateways.md`
3. `methodology/context/MMCF-Context-Coordination-and-Meta-Policy.md`
4. `methodology/context/MMCF-Conflict-and-Applicability-Profile.md`
5. `methodology/core/MMCF-Minimal-Working-Model.md`
6. `methodology/delivery/MMCF-Delivery-INDEX.md`
7. `reports/MMCF_REVISION_REPORT_2026-03-12.md`
8. `reports/PROJECT_REVISION_REPORT_2026-03-15.md`

## Contribution Format (Recommended)

For methodology PRs, include:
1. operational problem;
2. methodological change proposal;
3. impact on role/gateway contracts;
4. observability and metrics impact;
5. compatibility with CDM/FROR references.
6. note whether the proposal assumes tooling that already exists in `mmcf-code`
   or only future tooling.

## Access to Implementation Track

If you want to contribute to runtime code (`mmcf-code`), open an issue/PR in public docs with:
1. scope of intended work;
2. relevant experience;
3. proposed first milestone.

Maintainers can then coordinate collaboration mode for implementation work.

## Current Practical Note

1. `mmcf-docs` is already a substantial methodology corpus.
2. `mmcf-code` is useful today mainly through skills and governance pipelines,
   not through a populated application `src/`.
3. Contributors should avoid wording that suggests a fuller implementation
   layer than the repo currently contains.
