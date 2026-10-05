# progress-ring

A ring that fills to a value and holds it — the reveal animates, the value is data.

## Technique

Same `pathLength="1"` trick as draw-on-line, but the dasharray encodes the **value**, not the reveal:

```svg
<circle class="arc" pathLength="1" stroke-dasharray="0.7 0.3" stroke-dashoffset="0"
        transform="rotate(-90 120 80)" … />
```
```css
.arc { animation: fill 1.6s cubic-bezier(.4,0,.2,1) .3s both; }
@keyframes fill { from { stroke-dashoffset: 1; } }
```

`dasharray="value remainder"` draws exactly `value` of the normalized circumference; `rotate(-90)` starts the arc at 12 o'clock. The attribute state is the finished ring, so CSS-less renderers show the correct value — the animation only sweeps the reveal.

## Failure modes we hit

- Animating `stroke-dasharray` instead of dashoffset makes the *value* animate (ring grows past its number then snaps back). Animate the offset; keep the value in the attribute.
- Forgetting `stroke-linecap="round"` makes low values (<5%) vanish: the round cap is what keeps a sliver visible.
- Without the rotate, the arc starts at 3 o'clock and every reader's eye re-anchors; 12 o'clock is the convention for a reason.
