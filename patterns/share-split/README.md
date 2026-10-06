# share-split

A 100% stacked share bar whose segments grow in from the left, one after
another — the "here is the composition of this column" moment. Used in
harness-atlas as the per-capability census row above the grid.

## Technique

Each segment is its own `<rect>` laid out end to end (x = running sum of the
shares). Animate `transform: scaleX()` from 0 with the origin pinned to the
segment's **left** edge, staggered so the bar reads as assembling rather than
appearing:

```svg
<rect class="seg s1" x="20"  y="48" width="228" height="18" fill="#FE9106"/>
<rect class="seg s2" x="248" y="48" width="96"  height="18" fill="#FEAF12"/>
```
```css
.seg { transform-box: fill-box; transform-origin: left;
       animation: split .7s cubic-bezier(.4,0,.2,1) both; }
.s2 { animation-delay: .35s; } /* stagger per segment */
@keyframes split { from { transform: scaleX(0); } }
```

## Traps

- **`transform-box: fill-box` is mandatory.** Without it the origin is the
  SVG canvas origin and every segment grows from x=0, sliding over its
  neighbours instead of unfolding in place. (Same trap as `bar-grow`, on the
  other axis.)
- **Segments must not overlap during the animation.** Because each segment
  scales about its own left edge, a segment mid-growth occupies only part of
  its slot — the gap is the point; it closes exactly when the segment lands.
  Do not "fix" the gap with negative delays.
- **Shares must sum to the track width.** The pattern shows proportions; a
  rounding error leaves a visible sliver at the right end. Compute widths
  from one total, not from rounded percentages.
- **Color-blind safety:** the segments here differ in lightness as well as
  hue (bright → dark). If your palette is hue-only, add a label per segment
  or a pattern fill; the animation does not carry the meaning.

## Degradation

- Sanitizer-hostile environments (GitHub README): the CSS animation is
  stripped and the final stacked bar still renders — the composition is the
  payload, the assembly is garnish.
- `prefers-reduced-motion`: segments render at full width immediately.
