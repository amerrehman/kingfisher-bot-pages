# Ladder Geometry

This document defines the single-layer ladder invariants for part `A` and part `B`.

## Priorities

1. Correct pending-order placement.
2. Continuation of the extended ladder.

Continuation must never violate placement correctness.

## Divider

For each batch, `B idx=1` is the fixed divider between the two ladder sides.

- `A` orders must stay strictly on the `A` side of that divider.
- `B` orders must stay on the `B` side of that divider.
- `A` must never be placed on the divider price.
- `A` must never cross through the divider into `B` territory.

## Order Role

Slot identity is defined first by geometry:

- `A` orders must stay on the `A` side of the divider.
- `B` orders must stay on the `B` side of the divider.

For live execution, slot role is still constrained by the divider:

- `A` continuation must remain an A-side limit order.
- `A` must still remain strictly on the `A` side of the divider.
- `B` must still remain on the `B` side of the divider.

Direction by base side:

- Base `BUY`:
  - `A` prices must be strictly greater than `B idx=1`.
  - `B` prices must be less than or equal to `B idx=1`.
- Base `SELL`:
  - `A` prices must be strictly less than `B idx=1`.
  - `B` prices must be greater than or equal to `B idx=1`.

## Continuation

When continuation is needed:

- Target `#1` wins over target `#2`.
- Extend `A` only while the next `A` rung still remains on the `A` side of the divider.
- If the next `A` rung would touch or cross the divider, do not place it.
- If the next `A` rung cannot remain a valid A-side limit before the divider, do not place it.
- Continue the ladder by extending `B` farther outward only after the legal `A` side has been
  exhausted.
- After a frontier `B` fill, refill companion `A` with the simplest live-gap rule first:
  if live `A` still exists, use the nearest missing live `A` rung;
  if live `A` has been fully consumed, continue from the outer known `A` frontier instead of
  rebuilding `idx=1/2`;
  then allow outward scanning for the first legal placement.
- Do not anchor `A` continuation to special-case inferred indices from recently filled `B` rungs.
- Let the placement helper decide whether the candidate `A` rung is still valid under divider and
  order-type constraints.
- If the first candidate `A` rung is not placeable at current market, keep scanning outward until
  the first valid `A`-side rung is found or the configured cap is reached.
- `B` continuation state must remember the highest `B` rung already created in the batch, even if a
  live/fill snapshot briefly misses an auto-hedged rung between passes.

## Live B Geometry

Auto-hedged `B` orders are part of the live `B` ladder geometry.

- They must be included when deciding the current `B` frontier.
- They must be included when deciding the next `B` index.
- Recovery logic must not treat `B` as exhausted while live or freshly-created `B` rungs still exist.

## Practical Review Checklist

When reviewing logs, these are the failure signs:

- `single-layer extend A` appears at the same price as `B idx=1`.
- An `A` continuation appears beyond the `B idx=1` divider.
- Recovery says exhausted `B` while live `B` hedge/continuation orders still exist.
- Continuation resumes on the wrong side instead of extending the correct frontier.
