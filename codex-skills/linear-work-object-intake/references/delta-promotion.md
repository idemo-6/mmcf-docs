# Delta Promotion Reference

Derived from:

- `mmcf-docs/methodology/MMCF-Delta-Registry-and-ChangeFlow-Promotion-Profile.md`

Use this reference when the request may still be upstream of terminal issue
materialization.

## Core Route

`Event -> Delta -> DeltaRecord -> DeltaRegistry -> Qualification/Triage -> PromotionDecision -> Queued ChangeFlow -> Todo -> CF1..CF6`

## What Matters Here

1. `DeltaRegistry` is not the queue of terminal issues.
2. A raw `DeltaRecord` is not a terminal issue.
3. Terminal issue materialization starts only after
   `PromotionDecision=promote`.
4. `Queued` is the pre-start state of an already materialized terminal flow.
5. `Backlog` must not replace the methodology boundary between upstream delta
   handling and downstream terminal execution.

## Intake Implications

1. Do not create a terminal issue just because a delta was recorded.
2. Do not pretend a coordination issue is the same thing as a `DeltaRegistry`
   entry.
3. Several delta records may still collapse into one terminal flow if they
   converge to one carrier contour and one base intent line.
4. `drop`, `park`, `merge`, and `split` do not create terminal issues.
