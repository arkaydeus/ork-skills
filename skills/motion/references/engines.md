# Motion engines

Pick the lightest engine that can make the shot. All of them use the same capture, look, fix loop. Licences change: check current terms before a client delivery.

British English throughout (colour, organised, towards, artefact).

Capture for HTML scenes is `references/render.md`. Timing tokens are `references/maths.md`.

## Decision

1. 3D, or real light and materials? **three.js**. Give CAMERA a **30–50°** FOV.
2. One idea, type and shapes, under about **10 seconds**? **Plain HTML**.
3. Complex 2D timelines, split text, SVG morph or line draw? **GSAP**.
4. Long, data-driven, captioned, or many variants? **Remotion** (licence permitting) or **HyperFrames**.
5. Already a React app, and the piece is live UI rather than a video? **Motion**.
6. Unsure? Propose two engines with trade-offs, then choose.

Do not start in three.js for a 2D dashboard. Do not start in Remotion for a 6-second chrome loop. Do not pick Motion when you need an MP4: it has no video renderer.

## How the picture gets made

These are job shapes, not extra libraries.

| Shape | What it is | Use | Do not use |
| --- | --- | --- | --- |
| Drawn in code | HTML canvas, SVG, or CSS. Zero or tiny dependencies | Interactive UI loops, pixel-true chrome, anything that must be fully deterministic | Organic faces |
| Composition framework | React compositions driven by a frame number | SaaS explainers, product tours, design-system showcases, captioned variants | Tiny loops where the React toolchain is the whole cost |
| Overlay | A character video model for faces and bodies; redraw sharp code-UI on top | Talking head plus a real interface | Letting the video model draw the UI |
| Texture only | Abstract generated plates | Backgrounds, atmosphere | Type, logos, UI, grids, product edges |

An overlay job still delivers the UI through drawn-in-code or a composition framework. The overlay must be seekable and composited, not hoped for inside the generated plate.

## Engine table

| Engine | Best for | How it seeks | Licence (verify) |
| --- | --- | --- | --- |
| Plain HTML/CSS | A single idea under 10 s: type, shapes, UI mock-ups | Pause every animation and set `currentTime` | None |
| GSAP | Complex 2D timelines, split text, SVG morph and line draw | One paused timeline; `tl.seek(t)` | Free, including plugins (SplitText, MorphSVG, DrawSVG, ScrollTrigger). The only restriction: do not build a no-code animation tool that competes with Webflow. Verify current terms. |
| Motion (formerly Framer Motion) | Live React or vanilla UI with decent springs. No video renderer | Pausable controls; set time from `t` | MIT |
| three.js | Materials, cameras, chrome, glass, product shots | Your `render(t)` sets object state and calls `renderer.render` | MIT |
| Remotion | Longer React videos, data, templates, captions | Built in via `useCurrentFrame()`. Own CLI, not `capture.mjs` | Free for individuals and companies of up to 3 people; larger companies need a company licence |
| HyperFrames | HTML compositions meant to be written by agents | Frame-accurate in its own renderer; follow its docs | Apache 2.0 |

Remotion scaffold when a composition framework is the job:

```bash
npx create-video@latest motion-lab
cd motion-lab
npx remotion skills add
```

Then still forbid timers. Frame number is the only clock.

## Seek hooks

GSAP:

```js
const timeline = gsap.timeline({ paused: true });
timeline.from('.headline', { y: 32, opacity: 0, duration: 0.5, ease: 'power4.out' });
window.seek = (t) => { timeline.seek(t); };
```

Plain CSS (Web Animations API):

```js
window.seek = (seconds) => {
  for (const anim of document.getAnimations()) {
    anim.pause();
    anim.currentTime = seconds * 1000;
  }
};
```

three.js: own `render(t)` that assigns rotations, camera, and materials, then `renderer.render(scene, camera)`. Use `preserveDrawingBuffer: true` so Playwright can screenshot the canvas. Pin the import map:

```html
<script type="importmap">
{ "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"
} }
</script>
```

In r160, `TextGeometry` extrusion depth is the `height` option, not `depth`. Set `window.__ready` only after fonts, textures, and `PMREMGenerator` environment maps have finished. Keep `--enable-unsafe-swiftshader` in the capture script or headless WebGL goes black.

A looping three.js body looks like this: normalised `phase`, whole-number turns, camera on a closed orbit.

```js
function render(t) {
  const phase = (t % DURATION) / DURATION;
  knot.rotation.y = phase * Math.PI * 2;
  camera.position.set(Math.sin(phase * Math.PI * 2) * 0.6, 0.4, 6);
  camera.lookAt(0, 0, 0);
  renderer.render(scene, camera);
}
window.seek = (t) => render(t);
```
