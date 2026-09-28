Create "Broken Glass — Cake Knife & Jiggler Cutters": a single self-contained HTML file (broken-glass-jello.html) with an interactive, physically simulated square of "broken glass" jello cake (jewel-coloured jello cubes set in a creamy white layer on a graham-cracker crust, a 90s potluck classic) that you can wobble like jelly, cut with a visible cake knife, and stamp with Jiggler cutters (star, heart, dino, lightning bolt) to lift the shapes out.

── Design principle ──
- The cake is the star and the only saturated thing on screen. The interface is quiet, calm and editorial, with a few restrained 90s nods. Nothing decorative moves, nothing sits behind the cake, and only one small UI accent (the coral active tab) is allowed near it.
- Visual inspiration: vintage analog-shop editorial design (cream paper, a huge printed-ink serif title, thin ruled grid, folder tabs, outlined tag pills). Use it as a mood, not a template: the layout, content and details are this project's own.

── Platform ──
- WebGPU + WGSL only, all JS/CSS inline, no libraries, no external assets except one Google Fonts stylesheet for Instrument Serif (display=swap, with a system serif fallback so the page still works offline). One <script type="module">.
- No WebGPU / no adapter → the loading screen's status line changes to a calm message ("This cake needs WebGPU.") with a specific explanation, and the page stays on the hero; if someone still enters the workspace, the same message sits in the stage area and the controls are disabled. Recover from device loss (up to two rebuilds); surface WGSL compile errors.
- Fixed 60 Hz simulation step with an accumulator, ¼-speed toggle, pause. Keys: Space pause, N nudge, R reset, H hand, K knife, C cutter, 1–4 cutter shapes; ↓ / Page Down / Enter goes from the hero to the workspace, ↑ / Page Up / Escape goes back.

── Page design (warm editorial, lightly 90s) ──
- Palette: warm cream paper (~#EEEAE0); the 3D floor sweep tone-maps to exactly the page colour. Ink is a soft charcoal (~#1E1B18), secondary text a warm grey (~#6B655C). One bright accent, coral (~#D9533B), used only for the active tool tab and the selected flavour's outline. A muted dusty teal (~#5E8C8A) only for the status badge.
- Typography: Instrument Serif (Google Fonts) for the title, caption, flavour heading, tool hints and "Get cooking"; a system serif (Iowan Old Style / Palatino / Times) for body text and notes; a system grotesk in bold (Helvetica Neue / Arial) for labels, numbers and pills; a system monospace (SF Mono / Menlo) only for tiny key hints and the spec-table labels.
- The page is two full-screen views in one file, stacked vertically; the document itself never scrolls. Moving between them is a single smooth vertical glide (~0.9 s, ease-in-out).

Screen 1 — Loading, then hero:
- Loading: only the title, "Broken Glass." in Instrument Serif at a modest size, centred on the page, with a small italic status line under it that updates with real setup steps ("Setting the jello…" while the WebGPU device and shaders start, "Cutting the cubes…" while the cake mesh builds, "Warming the lights…" while the first frames render). No spinner, no percentage.
- When everything is ready, the title grows smoothly (~1.2 s) from centred and small to huge, spanning the full page width at the top (fit its size to the viewport width), with a subtle printed-ink speckle (fine SVG feTurbulence mask at low density, so the letters still read solid from a distance). There is no top bar, icon or navigation.
- Then the intro row fades in, set a little lower than directly under the title (≈9 vh of space): on the left a short paragraph ("Broken Glass is a study of the potluck classic: cubes of cherry, lime, orange and berry-blue jello set in a creamy condensed-milk layer on a graham-cracker crust. Grab it and it wobbles. Slice it and every cut face becomes a new mosaic. Stamp it with a Jiggler cutter and lift the shape right out.") opening with "Broken Glass" in bold grotesk, underlined; on the right the caption in Instrument Serif italic, right-aligned: "Stained glass you can eat. / Wobbly on purpose. / A potluck favourite, circa '96."
- Centred at the bottom: "Get cooking" in Instrument Serif italic with a small down arrow under it that gently bobs. Clicking it, scrolling down, swiping up, or pressing ↓ / Enter glides to the workspace. The cake is not shown on this screen.

Screen 2 — Workspace (fills the viewport exactly and never scrolls, so drag and wheel always act on the cake):
- Left: the 3D stage, filling the whole left area, with no border or outline: the cake sits directly on the cream paper. Over it, top-left, a small Instrument Serif italic link "↑ Broken Glass" that glides back to the hero; top-right, the status badge pill ("WebGPU · Live / Paused / Unavailable", dusty teal, no blinking); bottom-left, the tool hint in Instrument Serif italic (changes per tool: Hand "Grab the cake and give it a wobble.", Knife "Drag across the cake to slice it.", Cutter "Click a flat piece to stamp a shape."). Toasts for misses appear over the stage in small italic.
- Right: a control panel ~420 px wide, separated from the stage by one thin vertical ink rule, made of stacked cells divided by thin rules. If it's taller than the screen, only the panel scrolls. Top to bottom:
  – Tool tabs: outlined folder tabs with rounded top corners sitting on a rule: Hand (H), Knife (K), Cutter (C), each with its key in tiny grey monospace. The active tab is filled coral with cream text.
  – Heading: the current flavour name in Instrument Serif (~40 px, e.g. "Classic") with the cream in small italic underneath ("Condensed milk"). It updates when the flavour changes.
  – Cutter shape: four outlined pills with line icons (Star, Heart, Dino, Bolt), active one filled ink, a small "1–4" hint. The cell is faded unless the Cutter tool is active.
  – Flavour: three small cards, each a square swatch drawn as cream with four tilted jello cubes, a bold name and an italic cream name underneath: Classic / Condensed milk, Tropical / Coconut, Berry / Vanilla. The selected card gets a coral outline. Switching recolours the cubes, cream, crust tint and shadow tint without resetting anything.
  – Feel: thin one-line sliders with hollow round knobs: Firmness (wiggly ↔ set) and Internal damping (bouncy ↔ syrupy), end labels in small italic.
  – Readouts: a small two-row spec table (monospace uppercase labels on the left, bold grotesk numbers on the right, hairline between rows): Pieces, and Cubes exposed (how many cube cross-sections are visible on cut faces). No mass, volume or energy readouts.
  – Actions: outlined pills "Give it a nudge", Reset, Pause; small tag-style toggles for ¼ speed and Show mesh.
  – Footer (pinned to the bottom of the panel): a tiny keyboard legend ("Space pause · N nudge · R reset") and a collapsible "Inside the experiment +" (physics, cutting, Jiggler cutters, rendering, plus a short note on the dessert's history: cubes of several flavours folded into a creamy base; also called stained glass, crown jewel or cathedral window dessert, and gelatina mosaico in Mexico; dates from 1950s Jell-O recipes and was a potluck staple for decades).
- Dashed SVG guide while drawing a knife stroke; toasts for misses, warm with one wink: "Place the cutter over the cake.", "As if! Lay that piece flat to stamp it.", "Too thin to cut there.", "That's plenty of pieces for the potluck."
- Camera frames the cake centred in the stage; drag on empty space orbits within limits, wheel zooms (limited), double-click resets, the camera eases after the cake if it is thrown. The simulation and rendering can idle while the hero is showing.

Responsive and motion:
- Under ~860 px: the hero intro stacks (paragraph, then caption left-aligned); the workspace stacks with the stage on top (~50% of the screen height) and the panel below, which scrolls on its own; tabs stretch full width; large touch targets.
- prefers-reduced-motion: the title and screen changes become simple fades, and the arrow doesn't bob.

── The cake ──
- A thick square serving cut from a 9×13 pan (≈2.6 × 2.6, height 0.9): slightly rounded vertical corners, softly bevelled top edge, a flat bottom.
- Zones computed from rest-space coordinates so they stay correct on every cut face:
  • Bottom: a graham-cracker crust layer (~0.15 thick), opaque warm tan with a crumbly speckle and a slightly uneven top boundary.
  • Body: a creamy, milky-white translucent jello layer (condensed-milk style) with soft scattering and a few tiny air bubbles.
  • Suspended in the body: 14–22 jewel-coloured jello cubes of varied size (0.25–0.5), randomly rotated and spread through the whole height, each clearly translucent and much more saturated than the cream around it. Cubes cut on any face show a crisp coloured cross-section, so every cut reveals a new mosaic.
  • Top: a thin glossy clear-jello skin.
- Flavours: Classic (cherry red, lime green, orange, berry blue in condensed-milk cream), Tropical (pineapple yellow, mango orange, strawberry pink in pale coconut cream), Berry (raspberry, grape purple, black cherry, blue raspberry in vanilla cream).
- Translucent candy rendering: refraction of the scene behind, view-ray thickness from a back-face depth pass, per-tap Beer–Lambert absorption (cubes absorb strongly in their own colour; the cream absorbs little but scatters a lot) capped by the back thickness, jittered blur, milky scattering in the cream, back-lit glow at thin edges and in cubes near the surface, Fresnel + GGX with a glossy wet top, procedural studio environment with softboxes; floor with PCSS-lite soft shadow tinted by the jello's colours plus contact AO from a top-down height map; MSAA ×4, HDR, PBR-Neutral tone mapping, sRGB, dither.

── Physics ──
- Tetrahedral soft body (per piece: 2D Delaunay of outline + lattice, extruded into prisms → tets), XPBD small steps with co-rotational per-tet shape matching (mass-weighted, quaternion rotation extraction), tet volume constraints, hard edge strain limits (≤1.8×), edge relative-velocity damping, floor contact with static/kinetic friction, settling damping, speed cap, NaN guard. The crust is much stiffer than the jello; cubes are slightly firmer than the cream around them, so the cake wobbles with visible internal motion.
- Hand tool: grab anywhere (ray pick on the skinned surface + forgiving fallback), soft patch attachment on a camera-facing plane, bounded reach, never pushed into the floor; wheel or second finger twists the held patch.
- All pieces live in one simulation (component id per particle) and one render mesh; pieces collide with each other (AABB-pruned particle pushes, a few mm per pass). Max 14 pieces. Render vertices, cubes and bubbles are skinned to tets by barycentric embedding.

── Pieces as distance fields (enables any shape and holes) ──
- Every piece is a signed-distance grid (≈1 cm cells) over the cake's shared rest plane. A cut or stamp is a boolean split of that region; then every corner is rounded with a morphological closing + opening (exact Euclidean distance transforms), the region is split into connected components, and each component becomes its own piece (crumbs below a minimum area are dropped).
- Meshing a piece: marching-squares outline loops (outer + holes), resampled, smoothed, snapped back to the zero set, outward normals; walls swept with the bevel profile; caps triangulated by Delaunay and filtered with an exact point-in-polygon test (holes stay open); crowded rim points at tight corners are merged with fan triangles so the surface stays watertight; the sim mesh uses the same loops.
- State transfer on every rebuild: new rest particles are embedded in the old tets and inherit interpolated position and velocity. The expensive rebuild is done the moment the user commits (release/click), so the swap mid-animation is only a few ms.

── The cake knife ──
- A procedurally modelled cake knife: a long, round-tipped brushed-stainless blade taller than the cake with a lightly serrated lower edge, and a chunky translucent acrylic handle in muted sea-glass teal (it refracts softly, a quiet 90s touch); drawn in the scene pass so the buried part is seen refracted through the jello; casts a shadow.
- Hovers with its edge under the cursor, lines up over the stroke while drawing (handle always on the left), and on release plays a ~1.1 s choreography: align → press (a collider pushes a rounded groove — the jello squashes and wobbles before it gives) → break-through (new pieces swapped in) → cut down, with a brief extra resistance at the crust → a wedge collider eases the faces apart and holds the lips down → lift, lips spring back with a jiggle.
- The stroke read on the surface it was drawn over defines a vertical blade plane; each crossed piece is split along it in its own rest frame (best-fit rigid frame of the deformed piece).

── Jiggler cutters ──
- Four cutters: Star, Heart, Dino (a chunky, simple brontosaurus silhouette, readable at small size), Bolt (lightning bolt). Each is a polygon rounded exactly like the jello pieces, so the ring and the hole it leaves always match. Mesh: a thin wall swept along the outline with a rolled rim on top, in frosted semi-matte plastic in soft, desaturated pastels (so they never outshine the cake).
- Cutter tool: the ring hovers under the cursor with the shape upright toward the viewer; a click stamps (a drag orbits instead). Only pieces lying flat can be stamped.
- On click: every flat piece under the ring is split into the part inside the wall and the part outside, leaving a thin sliver gap where the wall was; the outside part may now have a hole or fall apart into several pieces. Cubes the wall passes through show crisp coloured cross-sections on both sides.
- Animation: align → press (the ring's distance field drives a collider that presses a groove all round) → break-through (swap in the new pieces) → down to the board while the wall wedges inside and outside apart → the stamped shape pops up a little with a wobble → the ring lifts and waits higher up until the pointer moves, so the result is visible. Then switch to the hand and lift the Jiggler out of its hole; stamps, knife cuts and further stamps can be combined freely.

── Quality bar ──
- Smooth 60 fps on a mid-range laptop; no per-frame allocations except when pieces are rebuilt.
- A test hook on window (sim, camera, pick, planCut/startCut, planStamp/startStamp, advance(seconds), per-piece stats, showScreen('hero' | 'workspace')) so animations can be stepped and screenshotted.
- Calm editorial layout with a light 90s accent; the cake stays the hero on phone and desktop.

── Not in this version (planned next) ──
- A Decorate tool with whipped cream dollops, rainbow sprinkles and maraschino cherries that ride along with the wobbling jello. Don't build it now, but keep the tool tabs and the surface-embedding code easy to extend with a fourth tool.
