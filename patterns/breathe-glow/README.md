# breathe-glow

A core dot and two concentric rings pulse in staggered phase — "this thing is alive" without any motion.

## Technique

One opacity keyframe, three elements, staggered `animation-delay` (0 / .8s / 1.6s on a 3.2s cycle) so the pulse travels outward:

```css
.core, .ring { animation: breathe 3.2s ease-in-out infinite; }
@keyframes breathe { 0%,100% { opacity: .35; } 50% { opacity: 1; } }
```

Opacity-only animation is the cheapest thing a renderer can do; safe anywhere.

## Failure modes we hit

- Scaling `r` instead of fading opacity looks better on some screens but is not compositor-friendly and some SVG-in-img renderers snap it. Opacity is the portable choice.
- Without stagger the three circles pulse in unison and read as a blinking dot, not a breathing signal.
