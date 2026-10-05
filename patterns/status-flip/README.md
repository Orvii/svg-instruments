# status-flip

A status line that flips between healthy and degraded states on a loop — for dashboards, incident-post headers, or any place that needs "this thing has states" at a glance.

## Technique

Two stacked groups, mutually exclusive via complementary `steps(1)` keyframes — no easing, no crossfade, because status changes are discrete events and animating them smoothly would lie:

```css
.good { animation: good 6s steps(1) infinite; }
.bad  { animation: bad  6s steps(1) infinite; }
@keyframes good { 0%,60% { opacity: 1; } 61%,85% { opacity: 0; } 86%,100% { opacity: 1; } }
@keyframes bad  { 0%,60% { opacity: 0; } 61%,85% { opacity: 1; } 86%,100% { opacity: 0; } }
```

`steps(1)` holds each state flat and switches instantly at the percentage marks; the two keyframe sets are exact complements so exactly one group is visible at any moment.

## Failure modes we hit

- Using a crossfade (ease) between states reads as "both are half true" — wrong semantics for status.
- If the two keyframe sets drift out of complement (a typo in one percentage), you get frames with both visible or neither; keep them generated from the same numbers.
- Reduced motion: freeze on the *healthy* state (`.bad { opacity: 0 }` in the reduce block) — a frozen "degraded" banner on a static page is a false alarm with infinite dwell time.
