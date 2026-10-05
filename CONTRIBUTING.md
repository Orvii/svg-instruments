# Adding a pattern

A pattern is one effect, one file, one README. The bar is the library's three rules plus one: someone must learn something from the failure modes.

## Checklist

- [ ] `patterns/<kebab-name>/pattern.svg` — self-contained: no `<script>`, no external `url()`/`@import`/`xlink:href`, no event handlers.
- [ ] Initial visible state = finished state, set via **attributes**; the animation's `from` keyframe supplies the start. CSS-less renderers show the completed picture.
- [ ] `@media (prefers-reduced-motion: reduce)` zeroes every animation in the file.
- [ ] `patterns/<kebab-name>/README.md` — technique (the 2-4 lines that matter), then **failure modes we hit** (at least two, concrete: what broke, what it looked like).
- [ ] Added to the catalog table in `README.md` and to `demo.md`.
- [ ] Viewed in both color schemes if the pattern is theme-sensitive; viewed with reduced motion on.

## What we will not accept

- SMIL-only patterns without a stated reason (CSS keyframes are the house style; see README "Why not SMIL?").
- Patterns whose only content is a gradient — gradients are fills, not instruments.
- A README that describes the effect without the failure modes. The failure modes are the research; the SVG is the demo.
