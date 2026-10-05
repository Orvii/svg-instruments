# draw-on-line

A polyline draws itself left to right, then its area fill fades in under it.

## Technique

`pathLength="1"` normalizes the path's length to 1, so `stroke-dasharray="1"` plus a dashoffset keyframe from 1 to 0 draws any path without measuring it:

```svg
<path class="draw" pathLength="1" stroke-dasharray="1" stroke-dashoffset="0" d="…" />
```
```css
.draw { animation: draw 2.2s cubic-bezier(.4,0,.2,1) .3s both; }
@keyframes draw { from { stroke-dashoffset: 1; } }
```

## Why the attribute says `stroke-dashoffset="0"`

Rule 2 of the library: the *attribute* holds the finished state (offset 0 = fully drawn). The `from` keyframe supplies the start (offset 1 = invisible). A renderer with no CSS support ignores the keyframe and shows the completed line. If you instead set `stroke-dashoffset="1"` in the attribute and animate `to { 0 }`, CSS-less renderers show a blank panel.

## Failure modes we hit

- Animating `stroke-dashoffset` without `pathLength` requires the real path length; get it wrong and the line draws partially or twice. `pathLength="1"` removes the measurement entirely.
- `animation-fill-mode: both` matters: without it the line flashes fully drawn during the delay, then jumps to hidden.
