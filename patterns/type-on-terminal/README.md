# type-on-terminal

Terminal lines type in one after another; each line's cursor blinks while it is the active line, then the last cursor keeps blinking forever.

## Technique

Typing is a `clip-path: inset(0 100% 0 0)` → `inset(0)` reveal with `steps(n)` timing, n ≈ character count, so glyphs appear discretely instead of sliding:

```css
.t1 { animation: type .8s steps(10) .5s both; }
@keyframes type { from { clip-path: inset(0 100% 0 0); } }
```

Cursor handoff: cursors 1 and 2 run the blink animation **once** (`iteration-count: 1`) starting at their line's delay, and their base `opacity` is 0 — so after their single blink cycle they vanish and the next line's cursor takes over. The last cursor uses `infinite`.

## Failure modes we hit

- `steps()` count must roughly match visible character count; too few steps and words pop in chunks, too many and it looks linear.
- Without the base `opacity: 0` on finished cursors, all three cursors stay visible after typing — the terminal looks broken.
- Reduced-motion must also zero the cursors (`opacity: 0`), otherwise a frozen mid-blink cursor sits on every line.
