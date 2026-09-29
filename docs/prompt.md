Create "Broken Glass — Cake Knife, Jiggler Cutters & Toppings": a single self-contained HTML file (index.html) with an interactive, physically simulated square of "broken glass" jello cake (jewel-coloured jello cubes set in a creamy white layer on a graham-cracker crust, a 90s potluck classic). You can wobble it like jelly, slice it with a chef's knife, stamp shapes out with Jiggler cutters (star, heart, dino, lightning bolt), decorate it with whipped cream, cherries and sprinkles, then serve the pieces you pick on a platter, plate or cup and send someone a short video of them landing and jiggling.

── Design principle ──
- The cake is the star, and toppings are part of the cake. The interface is quiet, calm and editorial, with a few restrained 90s nods. Nothing decorative moves, nothing sits behind the cake, and only one small UI accent (coral) is allowed near it.
- Visual inspiration: vintage analog-shop editorial design (cream paper, a huge printed-ink serif title, thin ruled grid, folder tabs, outlined tag pills). Use it as a mood, not a template.

── Platform ──
- WebGPU + WGSL for rendering; the physics runs on the CPU in plain JavaScript. All JS and CSS inline, no libraries, no external assets except one Google Fonts stylesheet for Instrument Serif (display=swap, with a system serif fallback). One <script type="module">. Deploys as-is to GitHub Pages.
- No WebGPU / no adapter → the loading screen's status line becomes a calm message ("This cake needs WebGPU.") with a specific explanation, and the page stays on the hero; the stage shows the same message and the panel is disabled. Recover from device loss (up to two rebuilds); surface WGSL compile errors.
- Fixed 60 Hz simulation step with an accumulator (at most 4 steps per frame), ¼-speed toggle, pause. Loading steps must not depend on requestAnimationFrame alone (fall back to a short timer), so the page also loads in a background tab.
- Keys: H hand, K knife, C cutter, D decorate; 1–4 cutter shapes (1–3 pick a topping while decorating); Space pause, N nudge, R reset; ↓ / Page Down / Enter enter the workspace, ↑ / Page Up / Escape go back (Escape leaves the share step first).

── Page design (warm editorial, lightly 90s) ──
- Palette: warm cream paper (#EEEAE0); the floor renders exactly the page colour where it isn't shadowed. Ink soft charcoal (#1E1B18), secondary warm grey (#6B655C). One bright accent, coral (#D9533B): the active tool tab, the selected card outline, the Share and Serve buttons, and the glow on picked pieces. Muted dusty teal (#5E8C8A) only for the status badge.
- Typography: Instrument Serif for the title, caption, flavour heading, hints, notes and buttons like "Get cooking"; a system serif (Iowan Old Style / Palatino / Times) for body text; a bold system grotesk (Helvetica Neue / Arial) for labels, numbers and pills; a system monospace (SF Mono / Menlo) only for tiny key hints and spec-table labels.
- Two full-screen views in one file, stacked vertically; the document never scrolls. Moving between them is one smooth vertical glide (~0.9 s).

Screen 1: loading, then hero
- Loading: only "Broken Glass." in Instrument Serif at a modest size, centred, with an italic status line under it that follows real setup steps ("Setting the jello…" while WebGPU and shaders start, "Cutting the cubes…" while the cake builds and a throwaway cut and stamp warm up the cutting code, "Warming the lights…" while the first frames render). No spinner.
- When ready, the title grows (~1.2 s) to span the full page width at the top, with a subtle printed-ink speckle (SVG feTurbulence mask). No top bar or navigation.
- Then the intro row fades in, a little lower (≈9 vh of space): a short paragraph opening with "Broken Glass" in bold, underlined grotesk; on the right the caption in italic, right-aligned: "Stained glass you can eat. / Wobbly on purpose. / A potluck favourite, circa '96."
- Centred at the bottom: "Get cooking" in italic with a gently bobbing down arrow. Clicking it, scrolling down, swiping up or pressing ↓ / Enter glides to the workspace.

Screen 2: workspace (fills the viewport and never scrolls)
- Left: the 3D stage with no border: the cake sits on the cream paper. Top-left an italic "↑ Broken Glass" link back to the hero; top-right the status badge ("WebGPU · Live / Paused / Unavailable"); bottom-left the tool hint in italic; small italic toasts over the stage for misses. A dashed SVG guide follows a knife stroke. A "Recording" pill appears while a video is being made.
- Right: a ~420 px control panel of stacked cells separated by thin rules; only the panel scrolls. Top to bottom:
  – Tool tabs: outlined folder tabs with rounded tops (Hand H, Knife K, Cutter C, Decorate D); the active tab is filled coral.
  – Heading: the flavour name in Instrument Serif (~40 px) with the cream in small italic underneath.
  – Cutter shape: four outlined pills with line icons (Star, Heart, Dino, Bolt); faded unless the Cutter is active.
  – Topping: three pills (Whipped cream, Sprinkles, Cherry); faded unless Decorate is active.
  – Share: one full-width coral button, "Share your creation".
  – Flavour: three small cards, each a cream square with four tilted cubes, a bold name and an italic cream name: Classic / Condensed milk (cherry red, lime, orange, berry blue), Tropical / Coconut (pineapple, strawberry, mango, pale pineapple), Berry / Vanilla (raspberry, grape, blue raspberry, black cherry). The selected card gets a coral outline. Switching recolours the cubes, cream, crust and shadow tint without resetting.
  – Feel: thin one-line sliders with hollow round knobs: Firmness (wiggly ↔ set, default 45%) and Internal damping (bouncy ↔ syrupy, default 35%).
  – Readouts: a small spec table (monospace uppercase labels, bold grotesk numbers): Pieces, Cubes exposed, Toppings.
  – Actions: outlined pills "Give it a nudge", Reset, Pause, "Clear toppings"; tag-style toggles for ¼ speed and Show mesh.
  – Footer: a tiny key legend and a collapsible "Inside the experiment +" (physics, cutting, cutters, rendering, and a note on the dessert's history: cubes of several flavours folded into a creamy base; also called stained glass, crown jewel or cathedral window dessert, and gelatina mosaico in Mexico; dates from 1950s Jell-O recipes).
- Camera frames the cake; drag on empty space (or right-drag) orbits within limits, the wheel zooms, double-click resets, and the camera eases after the cake if it's thrown.
- Under ~860 px: the hero intro stacks; the workspace stacks with the stage on top (~50% height) and the scrolling panel below; key hints hide; large touch targets. prefers-reduced-motion: the title and screen changes become fades, the arrow doesn't bob, and toppings appear without growth animations.

── The cake ──
- A thick square serving from a 9×13 pan: 2.6 × 2.6, height 0.9, rounded vertical corners (radius 0.14).
- Zones from rest-space coordinates, so they're correct on every cut face: a graham-cracker crust in the bottom 0.15 (opaque tan with crumbly speckle); a creamy, milky translucent condensed-milk body; 64 jewel-coloured jello cubes (half-size 0.09–0.145, random rotations, spread through the body and allowed to meet the outer walls so cubes show on the sides). Cubes cut on any face show a crisp coloured cross-section.
- Rendering: each jello pixel marches a ray through the piece's resting shape (the refracted view ray turned into the piece's rest frame). Find where it leaves the piece by stepping the distance field and refining; integrate the cream analytically between exact ray–box intersections with the cubes (cream scatters like milk, cubes absorb like stained glass); a ray reaching the crust picks up crust colour. What's behind shows through from a scene texture, bent a little. Add Fresnel, GGX highlights from a procedural studio environment with two softboxes, diffuse shaping by the surface normal, Khronos PBR Neutral tone mapping, sRGB and dither. Sample the distance field bilinearly in the shader, matching the CPU sampler.
- Floor: the page colour with soft Gaussian shadow blobs per piece (from each piece's particle spread and height) tinted toward the cube colours. MSAA ×4.

── Pieces as distance fields ──
- Every piece is a signed-distance grid (144 × 144, 2 cm cells) over the cake's shared rest plane. A cut or stamp is a boolean split; every corner is then rounded with a morphological closing then opening (radius 0.04) using exact Euclidean distance transforms. The result is split into connected components, and each becomes a new piece (crumbs under 0.02 area are dropped; a split leaving a side under 0.035 is refused as "Too thin to cut there."). Max 14 pieces.
- Meshing a piece: marching-squares loops, resampled (3.5 cm), smoothed, snapped back to the zero set and oriented with outward normals. Top and bottom caps from a Delaunay triangulation of the loop points plus a 7 cm interior grid, filtered by centroid and edge midpoints but always keeping edges between neighbouring outline points (so caps close cleanly against the walls). Walls are rings at 7 heights. Triangle winding is fixed so geometric normals point outward.
- Delaunay: Bowyer–Watson with a determinant in-circle test on counter-clockwise triangles and a tiny jitter.

── Physics ──
- Soft body per piece: points on a 0.2 grid inside the piece plus outline points every 9 cm (inset 1.2 cm), Delaunay-triangulated and extruded through 4 levels (0, 0.3, 0.6, 0.9) into prisms, each split consistently into 3 tets by vertex order.
- Solver (3 iterations per step): co-rotational per-tet shape matching (rotation from a warm-started quaternion, Müller-style extraction), Jacobi-averaged, with the crust layer stiffer; plus a weak whole-piece shape match that also gives each piece its rigid frame (centroid and rotation). Stiffness from Firmness (alpha = 0.1 + 0.6·firm²). Gravity, floor contact with friction, non-rigid velocity damping from Internal damping (keeps throws, calms the jiggle), a speed cap, settling damping and a NaN guard.
- Render vertices are embedded in tets: clamped barycentric weights plus a small leftover offset rotated by the piece's frame (robust on thin sliver tets).
- Pieces collide with each other's real shapes: soft-body points and the visible wall points of one piece are tested against the other piece's distance field (extruded 0–0.9, placed by its rigid frame), pushed out along the nearest face with friction, both ways. Pieces stack like food instead of sinking in or blending.
- State transfer on every cut or stamp: new particles are embedded in the old piece's tets and inherit position and velocity. The rebuild happens the moment the cut is committed (about 0.3 s for a big piece).
- Hand tool: grab anywhere on the surface (ray pick on the skinned mesh); nearby particles follow a camera-facing drag plane (reach 2.2, never into the floor); the wheel twists what you hold.

── The chef's knife ──
- Procedurally modelled: a brushed-steel blade (length 2.4, heel height 0.5) with a straight spine dipping slightly near the tip and a curved belly rising to meet it, thinning toward the edge; a steel bolster; a mint handle (#BFE3CF) in line with the blade with three dark rivets on each side. Drawn in the scene texture too, so the buried part shows through the jello.
- Hovers with its edge under the cursor, handle always on the left. While a stroke is drawn it lines up with the stroke. On release (~1.1 s): align with the heel just past the near edge of the cake (so the handle stays outside it) → press a groove that follows the curved edge → swap in the new pieces → cut down to the board while a wedge eases the faces apart → lift.
- The stroke's first and last hits on the surface define a vertical plane; each crossed piece is split along it in its own rest frame. Pieces not lying flat refuse with "Lay that piece flat to cut it."

── Jiggler cutters ──
- Star, Heart, Dino (a chunky brontosaurus), Bolt. Each polygon is rounded exactly like the jello, so the ring and the hole match. Mesh: a thin wall with a rolled rim in frosted pastel plastic (butter, dusty pink, sage, lavender).
- The ring hovers under the cursor, oriented toward the viewer; a click stamps (a drag orbits). Only pieces lying flat can be stamped ("As if! Lay that piece flat to stamp it.").
- Animation: align → press a groove all round → swap in the new pieces → down to the board while the wall mostly pushes the outside piece away (the stamped shape is only nudged) → the shape pops up gently → the ring lifts and waits until the pointer moves.

── Decorate: toppings ──
- Toppings are embedded in their piece's soft body like the surface itself (a rest-space point, normal and optional orientation), so they wobble with it and follow the piece through cuts and stamps (those in the cut gap are lost). Toppings that land on the table are stored in world space. Cherries and cream dollops are solid: if one would poke into a dish or the table, it pushes its piece away. Limits: 120 cream dollops and cherries, 2,400 sprinkles.
- Whipped cream: a vintage aerosol can held upside down just above the cursor: cream body, domed shoulder, metal rims, a long fluted white nozzle, and a round label with sunburst rays, a double ring and a "WHIPPED CREAM" banner (drawn procedurally, readable with the can inverted). Hold to spray: a thin stream shows, a piped rosette (eight twisting ridges, height 0.27) grows while you hold still, and moving while holding pipes a line of rosettes. Top-facing surfaces only.
- Cherries: a cherry hovers above the cursor to aim; click to drop it under gravity. Landing on cream, it sits upright on top; on bare jello it bounces, rolls and topples onto its side, then settles and rides the wobble; off the cake, it lands on the table. Look: glossy candy red (#FF1F35), radius 0.1, slightly wider than tall with a dimple, and a long thin curved red stem ending in a darker knob.
- Sprinkles: a vintage decors bottle held upside down above the cursor: squarish glass with rounded shoulders, a ridged blue screw cap with a ring of holes, a cream label with a red header and teal polka dots, and sprinkles settled toward the cap. Hold and shake: emission scales with how fast you shake, and the bottle swings with your motion. Each sprinkle (a rounded 90s rainbow jimmy) tumbles as it falls and sticks wherever it lands: top, sides or table.

── Share: serve it up ──
- "Share your creation" switches the panel to "Serve it up" (with "← Back to the kitchen"), in three steps:
  1. Pieces: click pieces on the table to pick them; picked pieces glow coral. Pick all and Clear. A single uncut cake is picked automatically.
  2. Dish: three drawn cards: Platter (pink milk glass on a pedestal), Plate (mint milk glass, scalloped rim) or Cup (lemon footed glass).
  3. Note (up to 120 characters), then "Serve & record".
- Dishes are thin shells of revolution (profiles spun around a centre) with glossy milk-glass shading. Pieces, cherries and sprinkles collide with the shell. The dish only exists during serving.
- Serving: the dish appears beside the cake and the camera glides to it while slowly circling. Pieces go biggest first: each lifts, is kept level, arcs over and drops onto the dish. They're placed side by side using their real footprints plus a small gap, and stack only when they don't fit. A toast warns "That's a lot of jello for one plate. Some might tumble!" A final tap makes everything jiggle together.
- The recording captures the drop and the jiggle as a 720 × 960 portrait video card (MP4 where supported, otherwise WebM): "Broken Glass." in Instrument Serif at the top, the dish scene in a framed square, and the note in italic underneath. Nothing else. It runs on the wall clock (about 5–9 s depending on how many pieces), with a timer backup so it finishes even if frames stall.
- A result sheet shows the video looping with: Send it (Web Share with the file, only where supported), Download, Serve again (puts the served pieces back on the table so you can try another dish), and Back to the kitchen.

── Quality bar ──
- Smooth 60 fps on a mid-range laptop (the physics takes about 3–4 ms per step with a dozen pieces); no per-frame allocations beyond rebuilding pieces and topping instances.
- A test hook on window.jello (sim, pieces, camera, pick, planCut/startCut, planStamp/startStamp, advance(seconds), stats, showScreen, setTool, setFlavour, setTopping, decorate(x, z, type), hoverAt, dropCherry, hold, shake, toppings, falling, setDish, enterShare, exitShare, pickAll, serve, sharePhase, onDish, recording) so every animation can be stepped and screenshotted.
- Known rough edges, acceptable for this version: a cut or stamp pauses for about 0.3 s while pieces rebuild; shadows are soft blobs rather than true contact shadows; top edges aren't bevelled; no pinch-zoom or two-finger twist on touch yet; serving a whole cake onto one dish overflows.
