# scanline-sweep

A thin vertical bar sweeps a panel left to right on a loop — instrument/radar feel over any static chart.

## Technique

The bar is a 2px rect inside a `<g>`; the group is translated, not the rect, so the keyframe stays a pure transform (cheap, compositor-friendly):

```css
.scan { animation: sweep 5s linear 1s infinite; }
@keyframes sweep { from { transform: translateX(0); } to { transform: translateX(520px); } }
```

The translate distance equals the panel width, so the bar exits exactly at the right edge and re-enters at the left on wrap.

## Failure modes we hit

- Animating `x` on the rect instead of `transform` on a group works but is not compositor-friendly and jitters on low-end renders.
- If the translate distance ≠ panel width, the bar visibly parks outside the panel at each wrap.
- Reduced-motion: the bar freezes at the left edge, which reads as a panel border — acceptable, but if you dislike it, also set `opacity: 0` in the reduce block.
