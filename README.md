# Broken Glass

An interactive, physically simulated 90s "broken glass" jello cake: jewel-coloured jello cubes set in a creamy condensed-milk layer on a graham-cracker crust. Wobble it, slice it with a chef's knife, stamp shapes out with Jiggler cutters, decorate it with whipped cream, sprinkles and cherries, then serve it on a dish and send someone a little video of it wobbling.

A single self-contained HTML file. The soft-body physics runs in JavaScript; rendering is WebGPU + WGSL.

## Run it

**Play it:** https://niftyclaudia.github.io/broken-glass/

Or open `index.html` locally in a browser with WebGPU (recent Chrome or Edge, or Safari 26+). No build step and no dependencies; the only external request is the Instrument Serif font from Google Fonts, with a system-font fallback.

If a browser blocks it from `file://`, serve the folder locally:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000/>.

## Controls

| Key | Action |
| --- | --- |
| H / K / C / D | Hand, knife, cutter, decorate |
| 1–4 | Cutter shape: star, heart, dino, bolt |
| 1–3 (while decorating) | Topping: whipped cream, sprinkles, cherry |
| Space | Pause |
| N | Give it a nudge |
| R | Reset |
| ↓ / ↑ | Enter the workspace / back to the hero |

- **Hand:** drag the cake to pull and wobble it. The mouse wheel twists what you're holding.
- **Knife:** drag across the cake to slice it.
- **Cutter:** click a flat piece to stamp a shape.
- **Decorate:**
  - *Whipped cream:* hold to spray from the can. A dollop grows while you hold still; move while holding to pipe a line.
  - *Cherry:* click to drop one. It stays upright on cream and topples onto its side on bare jello.
  - *Sprinkles:* hold and shake the jar. The faster you shake, the more fall, and they stick wherever they land, table included.
  - Toppings ride the wobble and stay with their piece when you cut or stamp it.
- **Share:** press *Share your creation*, click the pieces you want to serve (they glow), pick a platter, plate or cup, and add a note. *Serve & record* lifts each piece onto the dish, and the drop and jiggle are recorded as a short portrait video card to send or download. *Serve again* puts the pieces back so you can try another dish.
- Drag on empty space (or right-drag) to orbit. The wheel zooms, and double-click resets the camera.

## How it works

- **Pieces as distance fields.** Every piece is a signed-distance grid over the cake's resting footprint. A cut or stamp is a boolean split, corners are rounded with a morphological close/open (exact Euclidean distance transform), and each connected part becomes a new piece.
- **Soft body.** Each piece is a tetrahedral mesh (Delaunay of the outline and interior points, extruded into prisms and then tets), solved with co-rotational per-tet shape matching. The crust is stiffer than the cream. New pieces inherit position and velocity from the old one.
- **Rendering.** Each jello pixel casts a ray through the piece's resting shape. The cream scatters like milk, the cubes absorb like stained glass (exact ray–box intersections), and the scene behind shows through with a little refraction.

## Test hook

`window.jello` exposes `sim`, `camera`, `pick`, `planCut`/`startCut`, `planStamp`/`startStamp`, `decorate(x, z, type)`, `advance(seconds)`, `stats()` and `showScreen('hero' | 'workspace')`, so animations can be stepped and screenshotted.

## Docs

- [`docs/prompt.md`](docs/prompt.md): the full build prompt (design, physics, tools).
- [`docs/mockup.html`](docs/mockup.html): the layout mockup for the hero and workspace screens.

## Next

- Shorten the pause when a cut or stamp is committed.
- Softer shadows with proper contact, and bevelled top edges.
- Touch gestures: pinch to zoom and two-finger twist.
