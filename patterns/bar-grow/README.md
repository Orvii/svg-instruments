# bar-grow

Bars rise from a baseline, staggered, as if the chart is being measured in front of you. The pattern behind every "data appears" moment in a dashboard hero.

## Technique

Animate `transform: scaleY()` from 0 to the element's natural size, and pin the transform origin to the bar's own bottom edge:

```svg
<g class="grow g1"><rect class="bar" x="80" y="70" width="70" height="60" fill="#FE9106"/></g>
```
```css
.grow { animation: grow .9s cubic-bezier(.4,0,.2,1) both; }
.g1 { animation-delay: .2s; } /* stagger per bar */
.bar { transform-box: fill-box; transform-origin: bottom; }
@keyframes grow { from { transform: scaleY(0); } }
```

The rect's attributes are the finished bar (full height, correct y). The `from` keyframe supplies the collapsed start. Per library rule 2, a renderer with no CSS support ignores the keyframe and shows the completed chart — the right degradation for a data graphic.

## Why `transform-box: fill-box` is not optional

This is the trap. In SVG, `transform-origin` percentages and keywords resolve against the **viewport** by default, not the element. Without `transform-box: fill-box`, `transform-origin: bottom` means "the bottom of the whole SVG", so every bar scales from the panel's bottom edge: bars appear to slide up out of nowhere and overshoot their slot instead of growing in place. `fill-box` makes the origin resolve against each element's own bounding box, which is what "grow from my own base" actually means.

If you see bars that grow but from the wrong place — or that seem to translate as they scale — this is the line you are missing.

## Why the wrapper `<g>`

The animation class lives on a `<g>` around each rect while `transform-box` lives on the rect. Either placement works; separating them keeps the stagger classes (`.g1`–`.g4`) from fighting the origin rule when you restyle, and lets you animate a bar plus its label as one unit later without touching the origin declaration.

## Failure modes we hit

- **Missing `transform-box: fill-box`** — the failure above. First symptom: bars growing from the panel edge.
- **`transform-origin: bottom` alone on the animated element without the class on the right node** — origin and animation must resolve on the same element (or its child); putting `.bar` styles on the `<g>` while animating the rect (or vice versa) silently reverts to viewport origin in some renderers.
- **`scaleY(0)` in the attribute instead of the keyframe** — CSS-less renderers then show an empty chart. Attribute = finished state, always.
- **No `both` fill mode** — bars flash at full height during the stagger delay, then snap to zero. `both` holds the `from` state through the delay.
- **Linear easing** — bars arriving at constant speed read as a loading spinner, not a measurement. The slight ease-out (`cubic-bezier(.4,0,.2,1)`) is what makes it feel like data settling.
- **Stagger too wide** — past ~150ms between bars the chart reads as four separate events instead of one dataset. 200ms total across four bars is the tested ceiling here.

## Reduced motion

`@media (prefers-reduced-motion: reduce) { .grow { animation: none; } }` — the attributes already hold the finished chart, so disabling the animation is the entire fallback. No extra rule needed.

## Where it is used in the wild here

[context-file-evidence](https://github.com/Orvii/context-file-evidence)'s hero uses this pattern for its mean/median divergence figure — two bar pairs, one falling and one rising, which is the whole argument of that repo in one glance.
