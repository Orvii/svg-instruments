# dash-flow

A dashed rail flows along a pipeline while nodes pulse in sequence — reads as "work moving through stages".

## Technique

Flow is a looping `stroke-dashoffset` animation. The offset target must equal one dash period (`dash + gap`), here `6 + 6 = 12`… doubled to `-24` for a two-period loop, which keeps the motion seamless because the pattern repeats exactly:

```css
.flow { animation: flow 2.6s linear infinite; }
@keyframes flow { to { stroke-dashoffset: -24; } }
```

Node pulses are the same opacity keyframe with staggered `animation-delay` set inline per node (`style="animation-delay:.6s"`), producing the left-to-right wave.

## Failure modes we hit

- If the dashoffset delta is not a multiple of the dash period, the loop jumps visibly at wrap-around.
- `linear` timing is required; any easing makes the flow stutter each cycle.
- Staggered delays on a *finite* animation desync after the first cycle — pulses must be `infinite` or the wave collapses.
