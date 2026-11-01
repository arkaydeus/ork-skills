# Motion maths

Keep timing in one token file and use only those values. Good motion is a handful of numbers applied consistently.

British English throughout (colour, organised, towards, artefact).

## Caveat: springs

Do not transcribe a spring equation from a blog screenshot. Use the standard underdamped step response below, which is what the tested `motion.js` implements.

Treat any "interface motion presets" table (stiffness / damping per widget type) as a sketch unless you measured it yourself. Prefer a damping-ratio target, then derive damping from stiffness and mass.

Never step a spring with Euler (or any integrator that needs the previous frame's velocity). That breaks `seek(t)`: frame 812 would depend on 0–811.

## Easing

Use a small named set. The JavaScript implementations below follow Robert Penner's easing equations (MIT / BSD; see `NOTICE.md`). Pair them with a CSS `cubic-bezier` or a GSAP named ease of the same family. Linear is right only for constant mechanical motion (a spin, a ticker).

| Name | Feel | Use |
| --- | --- | --- |
| ease-out-expo | Fast start, long soft landing | Confident entrances |
| ease-out-quint | Slightly gentler landing than expo | Default for anything arriving |
| ease-in-out-quart | Ease at both ends | Moves that start and end at rest; camera moves |
| ease-in-expo | Slow start, then a rush | Exits accelerating out of frame |
| ease-out-back | About 10% overshoot | Once per shot at most |

GSAP names that sit close: `expo.out`, `power4.out` (quint), `power3.inOut` (quart), `expo.in`, `back.out`.

## Springs

A spring is stiffness `k`, damping `c`, mass `m`. The feel number is the damping ratio:

```text
ζ = c / (2 √(k m))
ω₀ = √(k / m)
```

| ζ | Feel | Typical use |
| --- | --- | --- |
| 1.0 or above | No overshoot. Above 1 gets sluggish | UI, text, anything that must not wobble |
| 0.7 to 0.9 | One small overshoot, settles quickly | Product shots, cards, logos landing |
| 0.4 to 0.6 | Visible bounce, a few oscillations | Playful characters, mascots, icons |
| below 0.3 | Jelly | Deliberate cartoon wobble only |

Peak overshoot of an underdamped step is about `exp(−ζ π / √(1 − ζ²))`. ζ = 0.5 is roughly 16% over. ζ ≈ 0.8 is about 1.5–2%, which is the "micro-overshoot" high-end UI is aiming at. If a brief says "2% overshoot", target ζ near 0.8, not 0.5.

Reference defaults (do not copy blindly):

- Remotion `spring()`: mass 1, stiffness 100, damping 10 → ζ = 0.5.
- react-spring default: tension 170, friction 26, mass 1 → ζ ≈ 0.997 (almost no bounce).

Opacity and colour should not bounce. Position and scale may.

### Closed form (step from 0 to 1, `t` in seconds since the spring started)

Underdamped, `ζ < 1`, with `ωd = ω₀ √(1 − ζ²)`:

```text
x(t) = 1 − e^(−ζ ω₀ t) [ cos(ωd t) + (ζ ω₀ / ωd) sin(ωd t) ]
```

Critically damped, `ζ = 1`:

```text
x(t) = 1 − e^(−ω₀ t) (1 + ω₀ t)
```

Overdamped, `ζ > 1`, with `s = ω₀ √(ζ² − 1)`, `r1 = −ζ ω₀ + s`, `r2 = −ζ ω₀ − s`:

```text
x(t) = 1 − (r2 e^(r1 t) − r1 e^(r2 t)) / (r2 − r1)
```

Return 0 for `t <= 0`. Map onto a range with `start + (end − start) * x(t)`.

### Starting stiffnesses (mass 1)

Pick ζ from the feel table, then `c = 2 ζ √k`. These `k` values are starting points, not a rate card:

| Element | k | Suggested ζ |
| --- | --- | --- |
| Buttons and toggles | 280 | 0.75 |
| Cards and panels | 160 | 0.80 |
| Headings and type | 110 | 0.95 |
| Mascots and badges | 200 | 0.45 |

## Durations, staggers, beats, camera

Durations in milliseconds for interface work sit roughly in 100–500 ms: 100 ms reads as instant, small changes 200–300 ms, larger travel can run longer. Film-style hero moves may take 600–1000 ms. A single move longer than a second usually feels slow on social feeds. Exits can be shorter than entrances.

Staggers: 30–80 ms between items. If there are many items, shrink the gap so the whole sequence stays under about 500 ms.

Beat grid: frames per beat = `fps × 60 / BPM`. At 60 fps and 120 BPM a beat is 30 frames (0.5 s). Put hero hits on beats, secondary hits on half-beats. Compute each beat from time; do not add a rounded frame count repeatedly or the grid drifts. See `references/sound.md`.

Holds: at least 0.5 s after a hero move; text still long enough to read twice (1.5 s is a floor); end card 1–2 s.

Camera: ease-in-out curves, travel 2–4% of frame. Parallax layers at about 1.0×, 0.5×, 0.2×. Do not run a large camera move and a large subject move together.

Accessibility that also makes better video: contrast at least 4.5:1 for normal text (3:1 large); nothing flashing more than three times in any one second. Those floors come from WCAG.

## motion.js (inline into scenes)

One source of truth. Functions return progress in 0…1. Tested in Node 20.

```js
export const FPS = 60;

export const ease = {
  outExpo(x) {
    return x >= 1 ? 1 : 1 - 2 ** (-10 * x);
  },
  outQuint(x) {
    return 1 - (1 - x) ** 5;
  },
  inOutQuart(x) {
    return x < 0.5 ? 8 * x ** 4 : 1 - ((-2 * x + 2) ** 4) / 2;
  },
  inExpo(x) {
    return x <= 0 ? 0 : 2 ** (10 * x - 10);
  },
  outBack(x) {
    const overshoot = 1.70158;
    const c = overshoot + 1;
    return 1 + c * (x - 1) ** 3 + overshoot * (x - 1) ** 2;
  },
};

export const MS = { micro: 100, fast: 200, base: 300, slow: 500, hero: 800, hold: 1000 };
export const STAGGER = { tight: 30, base: 50, loose: 80 };
export const frames = (ms) => Math.round((ms / 1000) * FPS);

export const tween = (t, start, dur, fn = ease.outQuint) =>
  fn(Math.min(1, Math.max(0, (t - start) / dur)));

export const dampingRatio = ({ stiffness, damping, mass = 1 }) =>
  damping / (2 * Math.sqrt(stiffness * mass));
export const dampingFor = (ratio, stiffness, mass = 1) =>
  2 * ratio * Math.sqrt(stiffness * mass);

export function spring(t, { stiffness = 100, damping = 10, mass = 1 } = {}) {
  if (t <= 0) return 0;
  const w0 = Math.sqrt(stiffness / mass);
  const z = dampingRatio({ stiffness, damping, mass });
  if (z < 1) {
    const wd = w0 * Math.sqrt(1 - z * z);
    return 1 - Math.exp(-z * w0 * t) *
      (Math.cos(wd * t) + ((z * w0) / wd) * Math.sin(wd * t));
  }
  if (z === 1) return 1 - Math.exp(-w0 * t) * (1 + w0 * t);
  const s = w0 * Math.sqrt(z * z - 1);
  const r1 = -z * w0 + s;
  const r2 = -z * w0 - s;
  return 1 - (r2 * Math.exp(r1 * t) - r1 * Math.exp(r2 * t)) / (r2 - r1);
}
```

Inside `render(t)`:

```js
const p = tween(t, 0.4, MS.base / 1000, ease.outExpo);
const s = spring(t - 1.2, { stiffness: 160, damping: dampingFor(0.8, 160) });
```

Sanity check (save beside `motion.js` and run with `node t.mjs`):

```js
import * as M from './motion.js';
for (const k of Object.keys(M.ease)) {
  console.log(k, M.ease[k](0).toFixed(3), M.ease[k](0.5).toFixed(3), M.ease[k](1).toFixed(3));
}
for (const c of [{}, { stiffness: 170, damping: 26 }, { stiffness: 100, damping: 20 }, { stiffness: 100, damping: 40 }]) {
  let max = 0;
  for (let t = 0; t < 3; t += 1 / 600) max = Math.max(max, M.spring(t, c));
  const stiffness = c.stiffness ?? 100, damping = c.damping ?? 10;
  console.log(JSON.stringify(c), 'ratio', M.dampingRatio({ stiffness, damping }).toFixed(3),
    'peak', max.toFixed(3), 'at3s', M.spring(3, c).toFixed(4));
}
```

Expect default spring peak near 1.16, react-spring-like `{stiffness:170,damping:26}` peak near 1.00, and critically damped `{stiffness:100,damping:20}` peak 1.00.
