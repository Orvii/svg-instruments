# staggered-reveal

A row of cards (or list rows) fades and rises into place one after another — the cheapest way to make a static diagram feel authored.

## Technique

One keyframe, one class, per-item inline delay:

```css
.item { animation: rise .6s ease-out both; }
@keyframes rise { from { opacity: 0; transform: translateY(16px); } }
```
```svg
<g class="item">…</g>
<g class="item" style="animation-delay:.25s">…</g>
```

`both` fill-mode holds each item invisible during its delay; without it every card flashes visible, then re-hides when its animation starts.

## Failure modes we hit

- Transform on `<g>` in SVG-in-img: fine in current renderers, but keep the translate small (≤20px) — large offsets clip against the viewBox edge mid-animation.
- Delays longer than ~150ms between items stop reading as a cascade and start reading as slowness. Four to six items at 200-300ms total span is the sweet spot.
- Reduced motion: with `animation: none` the items simply appear — the correct degradation, since the finished state is the attribute state.
