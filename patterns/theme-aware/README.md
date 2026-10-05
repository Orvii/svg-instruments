# theme-aware

One SVG file that dresses itself for light and dark readers — no `<picture>` wrapper, no duplicate assets.

## Technique

SVGs loaded through `<img>` on GitHub receive the viewer's `prefers-color-scheme`, so a media query inside the file's `<style>` can swap palettes:

```svg
<rect class="bg" width="600" height="180" fill="#F8F5F2"/>
<style>
  @media (prefers-color-scheme: dark) { .bg { fill: #0b0806; } }
</style>
```

The trick that keeps library rule 2 intact: **the presentation attributes carry the light palette**, and the CSS only *upgrades* to dark. A renderer without CSS shows the light version — a complete, correct still — instead of an invalid `var()` fill or a black void.

## When to prefer `<picture>` instead

If the two themes need different *geometry* (not just colors), or you must guarantee pixel-identical brand assets, ship two files and a `<picture>` wrapper. Media-query theming is for palettes; `<picture>` is for layouts.

## Failure modes we hit

- Putting both palettes only in CSS (no attributes) breaks CSS-less renderers: fills become invalid and default to black.
- Forgetting that `stroke` needs the same treatment as `fill`; a dark-mode panel with light-mode gridlines reads as a rendering bug.
- Testing only in your own theme. Flip the OS/browser scheme and look again — or you ship a dark-on-dark surprise.
