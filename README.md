# How Does an Aircraft Carrier Launch a Jet in Seconds?
82 s, 9:16 steam-catapult explainer (Three.js 0.170.0 via import map, WebGL). All animation state is a pure function of timeline time.

## Files (flat directory, each < 25 MB)
- `index.html` – the whole application
- `uss_nimitz_cvn-68_aircraft_carrier.glb` (16.5 MB), `boeing_fa-18f_super_hornet_-_free-compressed__2_.glb` (19.0 MB), `ocean_wave_-__wmaya__1_.glb` (9.6 MB, Draco), `table_mountain_2_puresky_2k.hdr` (5.2 MB, real Radiance RGBE)

## Run locally
`python3 -m http.server 8000` in this folder, open http://localhost:8000/ (ES modules do not work from file://). Needs a current Chrome/Edge/Firefox with WebGL2 and internet for the Three.js CDN (jsdelivr) and the Draco decoder.
Click **Sound** once to enable audio (synthetic Web Audio, not recordings). **Record WebM** restarts the timeline and downloads a `.webm` (canvas incl. captions/labels, plus audio if Sound is on). It is WebM, not MP4.

## Deploy to GitHub Pages
Commit all files to a repo, Settings > Pages > deploy from branch root. All URLs are relative.

## What the assets actually contain (inspected)
- Carrier: no textures, 43 meshes grouped by material (SketchUp/Collada export), no separate catapult, shuttle, JBD or island nodes. Units are inches (scaled 0.0254), Y up, waterline at y=0. Deck height, bow end, island and lane position are **auto-detected from geometry at load** (largest horizontal surface; override with `?deckY=&laneZ=`). Track, shuttle, holdback, JBD, pistons and water brake are procedural overlays, not cut into the deck.
- F/A-18: 8 meshes, no launch bar, no separate nose gear (gear-down and gear-up are whole meshes, toggled after launch). Launch bar and holdback are procedural; nose-gear position (~6.4 m ahead of origin) is an estimate.
- Ocean: Z-up, 98 vertex-animation frames, Draco, ~30-unit patch. Scaled up and flattened (14x, 0.18 height); a plain plane fills the distance.
- Cutaway: a clipping window opens in the carrier deck over a schematic pit (labelled "EDUCATIONAL SCHEMATIC").

## Known limitations
- Narration is on-screen captions only (no voice).
- Jet exhaust is soft deterministic particles, not heat-distortion refraction. No ambient occlusion or contact-shadow pass.
- Time is stretched: the 95 m stroke takes 6 s, and post-launch speed is eased so the jet stays in frame.
- Lane/island/bow detection is heuristic and may need `?laneZ=` / `?deckY=` tuning.

## Testing actually performed
Done: JS syntax check (node --check); scene intervals sum to 82 s with no gaps; state determinism, no early jet/shuttle motion, no early airborne, shuttle within track, no NaN cameras over a 0.05 s sweep; file sizes and relative paths; GLB/HDR header inspection.
**Not done:** no browser was available, so rendering, loading of the GLBs/Draco/HDR, camera framing, clipping window, audio, recording and UI were never run. Expect to tune placement and camera keys after the first real run.
