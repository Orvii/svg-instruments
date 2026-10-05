# Changelog

## [2026-10-05] - Pattern 10: bar-grow (+ gallery gap closed)

### Added
- `patterns/bar-grow/` — bars rise from the baseline via `scaleY` with `transform-box: fill-box`; README documents the viewport-origin trap (bars growing from the panel edge) and why the attribute holds the finished bar.
- `index.html` gallery: the `progress-ring` figure was missing from the live page despite existing as a pattern; added alongside bar-grow.

### Modified
- `hero.svg` aria-label: five -> ten patterns.
- `README.md` catalog row; `demo.md` section.

## [2026-10-05] - Initial release: nine patterns + live gallery

### Added
- Patterns: draw-on-line, type-on-terminal, dash-flow, scanline-sweep, breathe-glow, staggered-reveal, theme-aware, status-flip, progress-ring — each with pattern.svg + failure-mode README
- `demo.md` — in-repo gallery; GitHub Pages gallery at orvii.github.io/svg-instruments (repo root as site source; see Orvii/retractions 006 for the path lesson)
- Three library rules: no scripts/external refs; degrade to finished stills; honor reduced motion
- `hero.svg`, `CONTRIBUTING.md`, `LICENSE` (MIT)
