# FluidMatter Water

**Real-time Water Rendering System | Unity URP Technical Art**

A modular water surface built as a data-contract problem: one owner per quantity, one definition per
quantity, and an automated gate that measures the result instead of a screenshot that happens to look
right from one angle.

**Status:** W0 – W3.1 accepted (see [validation scope](#how-much-of-this-is-measured)) ·
W4.0 caustics implemented, **validation pending** · portfolio scene rendering, **art pass in progress** ·
research prototype, not a product.

![Two Gerstner waves meeting a shoreline: displaced surface silhouette, foam band following the crest, depth-driven absorption](media/showcase/w31_gerstner_two_wave_shoreline.png)

*Actual engine capture, debug mode 0 (final composite), 1280×720. Shows Gerstner surface displacement,
the shoreline band and the noise-broken foam layer, from the **accepted W3.1** validation rig. It is an
honest technical capture, not an art pass.*

*Why this is still the front image:* the project's portfolio scene has since been rendered, and its newest
frames are better-looking water — see
[Gallery](#gallery--newest-first). But the surrounding scenery in that scene is still a blockout, so those
frames are published as **art pass in progress** rather than promoted to cover. The cover is decided on the
look, not on what is newest.

---

## Gallery — newest first

Every image below is a real capture from this project's own engine runs. None is AI-generated, none is a
marketing render, and no diagnostic view is presented as finished appearance. Every caption says which
milestone produced it and whether it is **accepted** or **work in progress**.

### Water appearance — portfolio scene, newest verified result

`FMS_Showcase_Island`, rendered in the real Tuanjie editor, 1920×1080, wave phase frozen at `t = 14 s`. These
are the newest frames and the first ones with a completed environment reflection input: a procedurally
generated sky cubemap (`FMS_Showcase_SkyCube`, not a photographed HDRI) bound through the same profile →
binder → `_FMS_*` path the accepted build uses. Material read back at capture time:
`_FMS_WaveCount = 4`, `_FMS_ShorelineEnabled = 1`, `_FMS_FoamEnabled = 1`, `_FMS_FoamCoverage = 0.42`,
`_FMS_EnvironmentReflectionAvailable = 1`, `_FMS_DebugMode = 0`, **`_FMS_CausticsEnabled = 0`**.

| | |
|---|---|
| ![Island lagoon from the technical camera with the sky reflection input bound: foam rings around the shallows, displaced four-wave surface](media/showcase/showcase_island_reflection_on_technical.png) | **Lagoon, `Camera_Technical`, reflection input on.** Four Gerstner slots displaced, shoreline band and noise-broken foam around the shallows, and the sky now present in the reflection term instead of the surface having nothing to reflect. This is the **same camera and pose** as the [Development Showcase](#development-showcase--work-in-progress) frame below, so the two can be compared directly. <br> Showcase scene · mode **0 = final composite** · **Art pass / Work in Progress** — the water systems on display are accepted; the art pass around them has no acceptance milestone |
| ![Low grazing view across the bay toward the island, sky reflection dominating the water surface](media/showcase/showcase_island_reflection_on_hero.png) | **Bay, `Camera_Hero`, reflection input on.** A grazing-angle view: the Fresnel term goes to the reflection side, so the surface reads as sky. The honest cost of that is less seabed and refraction detail than the technical view — the reflection is environment content, not screen-space or planar reflection, and no ray tracing is involved. <br> Showcase scene · mode **0** · **Art pass / Work in Progress** · not published as the cover |

### Shoreline & foam — accepted W3.1

The W3.1 frames in this gallery come from the controlled validation rig (known geometry, 1280×720, no art
assets), which is what makes the measurements in
[How much of this is measured](#how-much-of-this-is-measured) mean something. The front image above is the
accepted W3.1 shoreline shot; the island frames above show the same shoreline and foam consumers inside a
scene.

| | |
|---|---|
| ![Wall shoreline close-up: foam breakup against a vertical contact](media/showcase/w31_wall_shoreline_closeup.png) | **Shoreline foam, close enough to read the structure.** The foam layer is thresholded noise, so it breaks up into cells instead of painting a uniform stripe; the band hugs the contact line and dies out on schedule. <br> Milestone **W3.1** · mode **0 = final composite** · **accepted** · validation rig |

### Depth & refraction

| | |
|---|---|
| ![Clear pool: depth-driven absorption gradient with the shoreline contact band](media/showcase/w31_flat_clear_pool.png) | **Beer–Lambert absorption + shoreline band.** A still, clear pool seen at a low angle: the colour shift from the contact edge into depth is thickness-driven absorption, and the thin bright line at the contact is the shoreline layer. <br> Milestone **W3.1** · debug mode **0 = final composite** · **accepted** · validation rig, not an art pass |
| ![Pool with two active Gerstner waves: absorption at the contact edge, displaced silhouette](media/current-build-composite.png) | **The accepted W3.0 build.** Two active Gerstner waves, depth-driven absorption at the contact edge, displaced silhouette. Kept here because it is the state before the shoreline consumer existed. <br> Milestone **W3.0** · mode **0 = final composite** · **accepted** · validation rig |

### Technical breakdown — same camera, one variable

| | |
|---|---|
| ![Two by two comparison: top row technical camera with the reflection input off then on, bottom row hero camera off then on](media/showcase/showcase_island_reflection_ab_same_camera.jpg) | **Reflection input off / on, same camera, one variable.** Top row = `Camera_Technical`, bottom row = `Camera_Hero`; left = `_FMS_EnvironmentReflectionAvailable = 0`, right = `1`. Exactly **one** shader property differs between the columns, and every differing pixel in both arms falls inside the water mask (0 outside it). With the input off the water body renders black — that is the honest statement that this path has no screen-space or planar fallback, not a bug hidden by cropping. Unlabelled and half-scale: pixels are nearest-sampled from the four 1920×1080 arms, nothing is painted or colour-graded. <br> Showcase scene · mode **0** · **Art pass / Work in Progress** · measured single-variable A/B |
| ![Same camera, two states: shoreline and foam consumers off (A) and on (B)](media/showcase/w31_consumers_off_on_same_camera.png) | **What the W3.1 consumer actually changes — same camera, nothing else different.** Panel A is the W3.0 state with the consumers off; panel B is the accepted W3.1 state with them on. The pair is one pose differing only in two enable flags, which is why it can be compared at all. <br> Milestone **W3.1** · mode **0** · **accepted** · *assembled figure: two unmodified captures, resized and labelled, no pixels painted* |
| ![One view rendered three ways: composite, boundary field encoding, validation preview](media/boundary-same-view-three-readings.png) | **One view, three readings.** A = what the build draws. B = the boundary *field* (R proximity, G validity, B normalised thickness). C = a validation tint. Panels B and C are **diagnostic views, not appearance** — they show the data a consumer reads, which is the point of the architecture. <br> Milestone **W3.0** · modes 0 / 10 / 11 · **accepted** · assembled figure |
| ![Gerstner two-wave superposition with the rig's analytic reference curves](media/gerstner-wave-mesh.png) | **Engineering proof, not a look.** Multi-wave superposition against the harness's CPU reference; the green/blue polylines are the rig's own analytic curves. Published because "the GPU agrees with an independent implementation" is a claim worth showing. <br> Milestone **W2.1** · mode **0** · **accepted** · technical evidence |

**What is deliberately missing from this gallery:** a beauty cover, a video, any caustics image, and any
underwater frame. The portfolio scene's older technical capture is kept in its own section below,
[Development Showcase — Work in Progress](#development-showcase--work-in-progress), because it predates the
reflection input and is not a finished look.

---

## Development Showcase — Work in Progress

The project's first portfolio scene, `FMS_Showcase_Island`, has been rendered in the real engine. This is the
oldest published capture from it, kept as the **before** state of the reflection work and labelled **work in
progress**, not as the cover.

![Shoreline and foam inside the experimental island scene — development showcase](media/showcase/showcase_island_technical_wip.png)

**Shoreline and Foam — Development Showcase** / Real Tuanjie URP render demonstrating the previously validated shoreline and foam system inside an experimental island environment. The showcase art pass is ongoing; caustics are disabled and the environment reflection input has not yet been completed in this capture.

Provenance, added so the label cannot drift: 1920×1080 single-frame capture from this project's own Tuanjie
editor on D3D11, byte-exact copy of the engine output (SHA-256 `66fd061306b5ad97…`, full value in
[media attribution](docs/media-attribution.md)). What this image **is not**: a Final Hero, an accepted W4.0
result, physically accurate caustics, final commercial art, or a GPU benchmark. It is kept here as the
**before** state: this frame was captured with no environment reflection input bound, and the two newer frames
in [Water appearance](#water-appearance--portfolio-scene-newest-verified-result) are the same scene and cameras
after that input was bound and measured. The art-directed cover candidate from this scene is rendered but
**not published** — the surrounding scenery is still a blockout, so it does not meet the cover bar. The scene's
underwater camera view is diagnostic only and is not part of the portfolio.

---

## What the system does

| Capability | What it means here |
|---|---|
| **Unity / Tuanjie URP** | Built and validated inside the URP forward path through a pipeline adapter; Core shader modules own generic maths and never touch engine services directly. |
| **Modular water rendering** | Depth, surface data, waves, optics, boundary and shoreline are separate modules with declared include-direction; ownership is checked by the gates, not by convention. |
| **Depth-based Beer–Lambert absorption** | Per-channel transmittance, monotonic non-increasing in water thickness, with one positive linear eye-depth convention feeding everything. |
| **Screen-space refraction** | Normal-derived offset with a bounded maximum, viewport-edge fallback, and explicit rejection of foreground geometry dragged below the waterline. |
| **Environment reflection + Fresnel** | Reflection vector + Schlick Fresnel composed against environment probe content. Labelled **preview** everywhere: it is not SSR and not planar reflection. |
| **Layered surface normals** | Geometry normal and shaded normal are separate members of one `SurfaceData` contract, so a normal map can shade a surface without silently changing its geometry. |
| **Gerstner waves** | Up to four authored slots (direction, amplitude, wavelength, speed, steepness); CPU-reference vs GPU agreement is graded, and the crest-folding limit is stated by the model rather than hidden. |
| **Shoreline / foam** | A depth-derived boundary field drives a shaped shoreline band and a world-space noise-thresholded foam layer. **Accepted at W3.1.** |
| **Profile / binding architecture** | Authored ScriptableObject profiles → binder → shader properties, through two backends (shared material cache and `MaterialPropertyBlock`) proven pixel-identical; hostile payloads sanitised at the system edge. |
| **Automated validation** | Headless in-editor gates render controlled rigs, compare against a CPU reference of the same maths, and write graded rows with expectations and verdicts. A pass can be re-derived from numbers. |

---

## Progress

| Stage | Public status |
|---|---|
| W0 – W3.1 (depth, refraction, absorption, reflection preview, surface normals, Gerstner, surface-data contract, boundary field, shoreline + foam) | **Accepted**, subject to recorded validation scope |
| W4.0 Caustics | **Implemented / Validation Pending** — present in the working tree, gated by its own CAU series, **not accepted**, and no image of it is published here |
| Showcase Island (portfolio scene, cinematic profile, three cameras) | **Art pass in progress** — the scene renders in the real engine. Four images from it are published: two newest 1920×1080 frames with the environment reflection input bound ([Water appearance](#water-appearance--portfolio-scene-newest-verified-result)), a same-camera A/B sheet assembled from the four arms, and the earlier no-reflection technical capture under [Development Showcase](#development-showcase--work-in-progress). The Hero cover candidate is rendered but **not published**: the scenery around the water is still a blockout. The underwater camera is **diagnostic only** |
| W4.1 / W5 | **Not started** |

---

## How much of this is measured

Not by looking at it and nodding.

- **W3.1 shoreline + foam:** `SHORELINE_TEST.csv` carries **342 graded rows, all PASS**, across twelve
  consumer gates plus calibration, beauty and performance arms. Among them: disabled-identity reproduces
  the W3.0 render with max channel difference `0.00000` over 469,236 owned pixels; the shipped shoreline
  curve is flat at both band endpoints (`0.00300` / `0.00298` slope per unit proximity) because a linear
  band reads as an outlined ring; the mask matches its own analytic shape to `0.00033` against a capture
  floor of `0.00195`.
- **Full regression:** **16 gates × 2 independent passes (sequential + isolated), all PASS**, exit code 0,
  every gate snapshotting 265 project files, `isolation_unresolved = 0` with **drifted 36 == restored 36**
  across the isolated pass, `errorCS = 0`, `shaderError = 0`, `ioError = 0`, `files_created_by_gate = 0`,
  no accepted asset left mutated.
- **W3.0 boundary field:** 15 gates × 2 passes, 3,343 graded rows, 0 FAIL.
- **Failures are kept, not deleted.** Six W3.1 failing runs are archived with their FAIL rows, so the
  record still shows fail → root-cause → fix → pass. Thresholds were never lowered to obtain a pass; where
  a bar was wrong, it was replaced by a *correct* bar and still graded against.

---

## Known Limitations

Stated as gaps, not as settled conclusions.

1. **Refraction is screen-space.** It can only move information already in the scene colour/depth buffers.
   It cannot recover occluded detail, and it is not ray-traced.
2. **Reflection is a preview path.** Environment probe + Fresnel. **There is no screen-space reflection and
   no planar reflection.** In this engine fork, probe content does not reach the shader's environment
   globals through the standard path in headless runs, which is why the reflection evidence is graded
   against a controlled probe rather than an arbitrary skybox. The portfolio scene's reflection input is a
   **procedurally generated** sky cubemap bound explicitly through that path: the water reflects environment
   content, it does not reflect the island, the rocks or the foam.
3. **Depth is view-axis thickness.** `thicknessEye` is measured along the view ray, not as the shortest
   distance to shore. Measured band width varies up to **4.98×** across a 30° view ramp — a published
   limitation, not smoothed over.
4. **Opaque scenery only.** Transparent objects never reach the depth texture, so the shoreline reports no
   contact against them — correctly, but silently. Water cannot touch water: one depth sample, one surface.
5. **Coverage is a field-level calibration, not a band-level promise.** Authored foam coverage matches the
   measured areal fraction of the whole noise field to ≤`0.001`, but a specific shoreline band measured
   `0.47995` against a predicted `0.60000`. An author who needs "x % of this shore has foam" must calibrate
   on that shore.
6. **No GPU performance figures exist.** The headless capture path costs as much as or more than the frame
   it captures, so no enabled/disabled shader delta was resolvable above harness noise. "Not measured" must
   not be read as "free". No FPS, no ms, no per-pass cost is claimed anywhere in this repository.
7. **Caustics are not accepted.** A caustics consumer exists in the working tree and is under its own
   validation series with unresolved failing rows. Nothing here claims physically accurate caustics, flux
   conservation, or a finished underwater look.
8. **There is no accepted beauty pass yet.** Every accepted image comes from a controlled test rig with known
   geometry and flat unlit scenery, chosen so the measurements mean something. The four images from the art
   scene (`FMS_Showcase_Island`) are published as **work in progress**, not as an accepted look: the newest two
   have the environment reflection input bound, the A/B sheet shows both states of that input, and the earlier
   single frame predates it. The scene's non-water art is still a blockout, which is why none of them is the
   cover.

---

## Not implemented / not claimed

Physically accurate caustics · flux-conserving caustics · ray-traced water · screen-space reflections ·
planar reflections · full underwater rendering · open-water crest foam · wakes · interaction foam ·
FFT / infinite ocean · flow maps · buoyancy · GPU benchmarks · cross-API certification · a 30-second demo
video.

A term appearing in a roadmap, a design note, or an in-progress source file is not evidence that it ships.

---

## Visual status — what is not here yet

**Beauty cover: NOT PUBLISHED.** The project's art-directed water scene (`FMS_Showcase_Island`) has been
rendered in the real engine, and four images from it are
[published above as work in progress](#water-appearance--portfolio-scene-newest-verified-result). The one
bounded art fix that was outstanding — the environment reflection input — has been bound and measured with a
single-variable A/B, and the result is the newest frames in the gallery. The Hero cover candidate still scores
below the bar for a cover, because the scene's non-water art is a blockout rather than finished environment
art, so it is **held back** instead of posted and the front image stays an accepted W3.1 technical capture.
Labeling these frames "work in progress" is the state, not a hedge.

**Underwater view: DIAGNOSTIC ONLY.** A third camera looks from below the surface. Its captures are kept as
internal failure evidence and the status token is `UNSUPPORTED_CAMERA_VIEW`. Nothing here claims full
underwater rendering, and no underwater frame is published.

**Motion demo: NEEDS_CAPTURE.** No actual-run clip is published.

**Caustics imagery: not published.** The W4.0 captures that exist are 256–512 px diagnostic field dumps
over a marker grid; none shows a water surface a viewer could judge, and the milestone is not accepted.

No generated or AI-produced imagery is used anywhere in this repository, and none is used to fill these gaps.

---

## Environment

Tuanjie 1.10.4 (Unity-compatible 2022.3.62t16) · URP 14.2.0-t1 · Direct3D 11 · HLSL / ShaderLab ·
C# editor automation · perspective validation rigs at 1280×720.

## Repo scope & rights

This repository is a **showcase**: selected documentation and rendered evidence only. Full project source,
scenes, shaders and engine/package content are not distributed here, and public visibility is not an
open-source licence. Third-party engine and pipeline names belong to their respective rights holders.
See [media attribution](docs/media-attribution.md) for per-image provenance and integrity hashes.

**More detail:** [technical overview](docs/technical-overview.md) ·
[validation & evidence](docs/validation-and-evidence.md) · [status & limitations](docs/status.md) ·
[media attribution](docs/media-attribution.md)
