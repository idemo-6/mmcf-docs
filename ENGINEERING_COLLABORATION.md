# Engineering Collaboration (MMCF)

Language: EN
Source of Truth: this file
Last Sync: 2026-03-06

## Purpose

Public entry point for methodology contributors working on MMCF operational and governance design.

## Primary Specialist Profile

1. Methodologists (change management, organizational design, governance)
2. Operational framework designers
3. Applied systems analysts

Engineering/architecture participation is welcome as supporting profile for implementation-readiness.

## Current Repository Model

1. `mmcf-docs` is public and contains methodology and operational profiles.
2. `mmcf-code` is currently private and contains implementation code.

## How External Methodology Contributors Can Contribute Right Now

1. Refine operational role/gateway contracts for `CF1..CF6`.
2. Propose machine-friendly trace/event schemas for phase/gateway telemetry.
3. Improve applicability/failure classification profiles.
4. Contribute implementation-readiness checklists for teams adopting MMCF.
5. Add domain-specific implementation examples consistent with MMCF canon.

## Technical Entry Points

1. `methodology/MMCF-Canonical.md`
2. `methodology/MMCF-Operational-Roles-and-Gateways.md`
3. `methodology/MMCF-Context-Coordination-and-Meta-Policy.md`
4. `methodology/MMCF-Conflict-and-Applicability-Profile.md`
5. `methodology/MMCF-Minimal-Working-Model.md`
6. `methodology/delivery/MMCF-Delivery-INDEX.md`

## Contribution Format (Recommended)

For methodology PRs, include:
1. operational problem;
2. methodological change proposal;
3. impact on role/gateway contracts;
4. observability and metrics impact;
5. compatibility with CDM/FROR references.

## Access to Implementation Track

If you want to contribute to runtime code (`mmcf-code`), open an issue/PR in public docs with:
1. scope of intended work;
2. relevant experience;
3. proposed first milestone.

Maintainers can then coordinate collaboration mode for implementation work.
