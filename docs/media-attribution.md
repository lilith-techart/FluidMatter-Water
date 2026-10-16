# Media attribution & publication scope

Every image in `media/` is an **actual capture from this project's own engine runs**, either on a controlled
validation rig or, for the portfolio-scene frames below, in the project's showcase scene. No image is
AI-generated, no image is a marketing render, and no diagnostic view is presented as finished appearance.
Copies retain the source pixels; where a figure is assembled, that is stated below.

> **Correction (2026-10-16):** The R03 `Reflection_ON` captures below
> were later found to have a **capture-state leak**: the runtime material
> clone had `FresnelF0=1` and `ReflectionStrength=1`, suppressing the
> transmitted water-colour term. These files remain **only as historical
> debug/provenance media**, not as correct final-composite beauty results.
> They have been removed from the featured README gallery. Claims below
> that R03 establishes natural transparent-water appearance or correct
> coexistence of absorption and reflection are **withdrawn**. Binding a
> cubemap and measuring a controlled A/B is not the same as proving a
> correct-looking water composite. The original pixels/hashes are not
> modified. See [LookDev 001](lookdev-2026-10.md).

## Historical R03 captures — reflection interface test, invalid as final water appearance

| File | Source capture | Milestone / status | Debug mode | Role |
|---|---|---|---|---|
| `showcase/showcase_island_reflection_on_technical.png` | `Assets/FluidMatter/Showcase/Screenshots/R03_Technical_Reflection_ON.png` | `FMS_Showcase_Island`; W3.1 systems accepted, **art pass in progress**, reflection input bound and measured | 0 = final composite | **WITHDRAWN AS BEAUTY EVIDENCE.** `Camera_Technical` (position `8, 12, 18`, euler `24.18927, 243.9464, 0`, FOV 38°, near/far 0.1/200), 1920×1080, wave phase frozen at `t = 14 s`. Same camera and pose as `showcase_island_technical_wip.png` below |
| `showcase/showcase_island_reflection_on_hero.png` | `…/R03_Hero_Reflection_ON.png` | same | 0 = final composite | Grazing-angle view from `Camera_Hero` (position `-16, 6, 8`, euler `9.625748, 122.0054, 0`, FOV 40°, near/far 0.1/160), 1920×1080, `t = 14 s`. **WITHDRAWN AS BEAUTY EVIDENCE** — historical capture only |
| `showcase/showcase_island_reflection_ab_same_camera.jpg` | `…/R03_Reflection_Comparison.jpg` | same | 0 = final composite | **Assembled figure, and it is the measurement.** 2×2: top row `Camera_Technical` OFF then ON, bottom row `Camera_Hero` OFF then ON, from the four 1920×1080 arms, half-scale nearest-neighbour, no text, no colour change. 1944×1104 |

What these three frames are claiming, and the number that backs it: they are two arms of a controlled A/B in
which **exactly one shader property differs** (`stateDiff_propertyCount = 1`,
`stateDiff_keys = "_FMS_EnvironmentReflectionAvailable"`), and the difference falls entirely on the water —
`differingOutsideMask = 0` pixels for both cameras, `insideShareOfDifference = 1`, mean channel difference over
all water pixels `1.021229` (hero) / `0.834653` (technical) across `929698` / `993534` water pixels. A separate
debug arm (`_FMS_DebugMode = 6`, not published here) encodes the sampled reflection vector itself and reads the
sky direction better than the ground direction by **4.77×** (hero) and **8.01×** (technical), which is what
distinguishes "reflecting the sky" from "brightening everything". Frame health on the published arms:
`nonBlackRatio = 1`, `magentaPixels = 0`, `nonFinitePixels = 0`. Subsurface coexistence is measured rather than
asserted: `3188` / `4989` chromatic water pixels sit above the stored sky cube's channel maxima
(`0.803922, 0.74902, 0.737255`) in the ON arms and `0` in the OFF arms. Foam highlights do not shrink
(`waterHighlights` ON/OFF = `929698/48662` hero, `993534/85114` technical). `_FMS_CausticsEnabled = 0` in both
arms — no caustics are present in any published image.

Run conditions from the same record (`R03_REFLECTION_AB.json`, `R03_REFLECTION_AB_LOG.txt`): Tuanjie 1.10.4 /
`2022.3.62t16`, batch mode, `NVIDIA GeForce RTX 4070`, `Direct3D11`, Gamma colour space, pipeline
`FMS_W0_URP`, shader `FluidMatter/FMS_Water` compiled, rendered material `FMS_W0_SharedCache_0` through the
`SharedMaterialCache` backend, profile `FMS_Showcase_Cinematic_Profile`, `missingScriptComponents = 0`,
renderer count 85, one directional light (`intensity 1.25`, colour `1.000,0.960,0.880`, euler
`44.99997, 335, 0`), `ReflectionProbe_Sky` still Realtime/HD with `texture = null` and ambient mode Trilight —
so the reflection these frames show comes from the explicitly bound cubemap, not from probe baking. Linear
buffer encoded with `LinearToGammaSpace` into 8-bit sRGB PNG. `sceneSaved = 0`; the scene file is byte-unchanged
across the whole run (`sceneSha256_before == sceneSha256_after == e0990dacb0d271f0…`).

Material read back at capture time: `_FMS_WaveCount = 4`, `_FMS_ShorelineEnabled = 1`, `_FMS_FoamEnabled = 1`,
`_FMS_FoamCoverage = 0.42`, `_FMS_DebugMode = 0`, `_FMS_EnvironmentReflectionAvailable = 1`,
**`_FMS_CausticsEnabled = 0`**, `envCube = FMS_Showcase_SkyCube`.

**About that cubemap:** `FMS_Showcase_SkyCube` is **procedurally generated by this project's own editor
generator**, not a photographed HDRI and not purchased content. The A/B record re-verifies it byte-for-byte
against the accepted generator run (`sky_assetSha256_actual == sky_assetSha256_acceptedRun == f6144c5f…`,
`SKY_ASSET_SHA_MATCHES_ACCEPTED_RUN = 1`), checks the seam (`sky_seamOverrunPositions = 0`, worst shared-edge
jump `0.117647` against a local one-texel gradient of `0.545098`), and confirms no blown-out texels
(`sky_blownOutTexels_above0935 = 0`). Captions call it "sky reflection" / "environment content" and never
imply the water mirrors the island, the rocks or the foam.

The `OFF` column is published *because* it looks bad: with no environment input, this preview path has nothing
to reflect and the water body renders black. That is the honest statement that there is no screen-space and no
planar reflection fallback, and it is the reason the reflection term is labelled **preview** throughout this
repository.

## Showcase Island — development showcase (work in progress, published 2026-10-16)

| File | Source capture | Milestone / status | Debug mode | Role |
|---|---|---|---|---|
| `showcase/showcase_island_technical_wip.png` | `Assets/FluidMatter/Showcase/Screenshots/B02_Final_Technical.png`, which is **byte-identical** to the engine's `B05_Technical_RAW.png` | `FMS_Showcase_Island` portfolio scene; W3.1 shoreline + foam accepted, **art pass in progress** | 0 = final composite | **Development showcase, not a cover.** The island bay seen from `Camera_Technical` (position `8, 12, 18`, euler `24.18927, 243.9464, 0`, FOV 38°, near/far 0.1/200), 1920×1080, wave phase frozen at `t = 14 s` |

Run conditions recorded by the capture harness itself (`B05_RENDER_HEALTH.json`, `B05_CAPTURE_LOG.txt`):
Tuanjie 1.10.4 / `2022.3.62t16`, batch mode, `NVIDIA GeForce RTX 4070`, `Direct3D11`, Gamma colour space,
pipeline `FMS_W0_URP`, scene root count 90, `missingScriptComponents = 0`, shader `FluidMatter/FMS_Water`
`isSupported = True`, `shaderHasError = False`, rendered material `FMS_W0_SharedCache_0` through the
`SharedMaterialCache` backend. Frame health: `nonBlackRatio = 1`, `magentaRatio = 0`,
`distinctColors8bit = 205`, `pixelsAuthoredVsWaterOff = 993534`. The linear-buffer capture was encoded with
`LinearToGammaSpace` into an 8-bit sRGB PNG. The harness never saved the scene (`sceneSaved = 0`).

Shader properties read back from the rendered material at capture time — this is what the image does and does
not contain: `_FMS_WaveCount = 4`, `_FMS_ShorelineEnabled = 1`, `_FMS_FoamEnabled = 1`,
`_FMS_FoamCoverage = 0.42`, `_FMS_DebugMode = 0`, **`_FMS_CausticsEnabled = 0`**,
**`_FMS_EnvironmentReflectionAvailable = 0` with `envCube = null`**. So the visible water has accepted
shoreline and foam, no caustics at all, and **no completed environment reflection input**. The scene's
`ReflectionProbe_Sky` is Realtime/HD with `texture = null` in this headless path, and ambient mode is
Trilight — which is precisely why the caption says the reflection input has not been completed rather than
implying a sky reflection is present.

Published under explicit owner decision D-01 as a Development Showcase image, and deliberately **not** as the
front-page cover. It is not a Final Hero, not W4.0 acceptance, not physically accurate caustics, not final
commercial art, and not a GPU benchmark. A Hero cover candidate from the same scene exists and is scored below
the publication bar, so it is withheld (see *What is deliberately absent*). It is kept in the README as the
**before** state of the reflection work: the three frames in the section above are the same scene and the same
two cameras after the environment reflection input was bound and measured.

## W3.1 — shoreline & foam (accepted milestone, published 2026-10-15)

| File | Source capture | Gate / milestone | Debug mode | Role |
|---|---|---|---|---|
| `showcase/w31_gerstner_two_wave_shoreline.png` | `Docs/Evidence/W3.1/screenshots/04_Gerstner_two_wave_shoreline_mode0.png` | W3.1 PHASE-14 portfolio set (`BEAUTY` arm, 342 graded rows all PASS) | 0 = final composite | Front-page capture: two Gerstner waves, displaced silhouette, shoreline band + foam. Validation rig, not an art pass |
| `showcase/w31_wall_shoreline_closeup.png` | `…/02_Wall_shoreline_closeup_mode0.png` | W3.1 `BEAUTY` arm | 0 = final composite | Closest readable view of foam breakup structure at a wall contact |
| `showcase/w31_flat_clear_pool.png` | `…/01_Flat_clear_pool_mode0.png` | W3.1 `BEAUTY` arm | 0 = final composite | Absorption gradient across a still pool with the shoreline line |
| `showcase/w31_consumers_off_on_same_camera.png` | `…/08a_Before_W3_0_consumers_off.png` (panel A) + `…/01_Flat_clear_pool_mode0.png` (panel B) | W3.1 `BEAUTY` arm | 0 = final composite | **Assembled figure.** Same camera, two enable states. Panels resized with Lanczos and labelled only |

Why the off/on pair is a legitimate comparison rather than two convenient cameras: the W3.1 report states
the before/after pair is one pool pose differing only in the two enable flags, and the published "after"
byte-stream is identical to portfolio shot 01 (`7e9f8ece…`), which proves panel B **is** that pose. The
first beauty pass actually failed on exactly this point (`beforeAfterDifferingPixels = 0`), was
root-caused to a gate inheriting boundary state from the previous arm, and is archived under `fails/`.

## W3.0 and earlier (previously published)

| File | Source capture | Gate / milestone | Debug mode | Role |
|---|---|---|---|---|
| `current-build-composite.png` | `PREVIEW_03_wall_side_waves2_mode0_composite` | W3.0 preview set | 0 = final composite | The accepted W3.0 build; validation rig, not an art pass |
| `boundary-same-view-three-readings.png` | `PREVIEW_03_wall_side_waves2_mode0_composite` + `_mode10_field` + `_mode11_preview` | W3.0 preview set | 0 / 10 / 11 | Annotated 3-up figure, same camera and same two active waves |
| `boundary-preview.png` | `PREVIEW_01_wall_side_mode11_preview` | W3.0 preview set | 11 = boundary validation preview | Diagnostic tint over composite; **not** shoreline foam |
| `boundary-field.png` | `BRD02_wall_outer0.10000` | W3.0 BRD-02 band geometry | 10 = boundary field encoding | R = proximity, G = validity, B = normalised thickness |
| `gerstner-wave-mesh.png` | `ger02_side_two_final` | W2.1 GER-02 multi-wave superposition | 0 = final composite | Technical evidence; the green/blue polylines are the rig's analytic reference curves |

## Integrity hashes (SHA-256, first 16 hex) of the published bytes

```
showcase/showcase_island_reflection_on_hero.png       1920x1080   869635 B   7c40893ccd7cdc22
showcase/showcase_island_reflection_on_technical.png  1920x1080   835021 B   509bde1ddb8eb7a6
showcase/showcase_island_reflection_ab_same_camera.jpg 1944x1104  189482 B   387507b40eac617f
showcase/showcase_island_technical_wip.png     1920x1080   920939 B   66fd061306b5ad97
showcase/w31_gerstner_two_wave_shoreline.png   1280x720   164448 B   7792b00c5d75e9fa
showcase/w31_wall_shoreline_closeup.png        1280x720   104426 B   51d9e864ee471d4e
showcase/w31_flat_clear_pool.png               1280x720    79406 B   7e9f8ece0934c5d3
showcase/w31_consumers_off_on_same_camera.png  1584x504    42726 B   69d391d05c08cfb7
current-build-composite.png                    1280x720   172417 B   812b9258136600ea
boundary-same-view-three-readings.png          2308x536   126834 B   f5df35192f4a5553
boundary-preview.png                           1280x720    38185 B   c0ce8d332899a72d
boundary-field.png                             1280x720    20951 B   5369aa2de7c5c7ee
gerstner-wave-mesh.png                          960x540   127474 B   b1229eebb60f68dd
```

Full values for the four portfolio-scene frames, since they are the published images whose claim depends on
being the engine's own bytes:

```
showcase_island_reflection_on_hero.png        7c40893ccd7cdc22b1a038abcacf1b46bf8a9b7d3247343d67bd26d1f1822791
showcase_island_reflection_on_technical.png   509bde1ddb8eb7a63c194da4949d97a10900d2fd81ec80dacff5c713b68931ec
showcase_island_reflection_ab_same_camera.jpg 387507b40eac617f277f6db6279800e321e815eb8cf855357b468a12cf88e9b6
showcase_island_technical_wip.png             66fd061306b5ad97eba3b6e0da469363df08e5cf1511bc57bfa49d268505d5f9
lookdev_a7_clear_lagoon.png                   275cbfd13673c3d9c4849216061289e7995c6a8ea6b95ab922602bd0eb9177ad
lookdev_a7_technical.png                      2e74cd7143a6342f24aa9027bb395895a6a7e888f62207da35335eeb3a8d668b
lookdev_c_stylized_shoreline.png              cc8f4eae9b29e485d9656b8d93561e05b85396652d1f954612274825d7348280
```

For the three newest frames the same digests are recorded by the capture harness itself in
`R03_REFLECTION_AB.json` (`R0_Hero_ON_png_sha256`, `R1_Technical_ON_png_sha256`,
`R03_Reflection_Comparison.jpg_sha256`), so the published file and the engine output are one and the same byte
stream. The two `OFF` arms (`b281dfa5c869fa3d…`, `beac3d703930dad9…`) are deliberately **not** published as
standalone images — a black-water frame is evidence, not a portfolio shot — but their pixels are visible inside
the comparison sheet.

The four W3.1 single-panel files are **byte-exact copies** of their named sources — not recompressed, so no
lossy or generative alteration is even possible. `current-build-composite.png`, `boundary-preview.png`,
`boundary-field.png` and `gerstner-wave-mesh.png` are likewise byte-identical copies. The two assembled
figures (`w31_consumers_off_on_same_camera.png`, `boundary-same-view-three-readings.png`) are unmodified
panels, Lanczos-resized and labelled, on a neutral canvas. No pixels were painted, retouched, or
substituted in any image here, and every assembled figure says so in its caption. `showcase_island_technical_wip.png`
is also a byte-exact copy, made with `cp` and confirmed by `cmp` against its source in the development tree.
The two newest PNGs (`showcase_island_reflection_on_hero.png`, `showcase_island_reflection_on_technical.png`)
were copied and `cmp`-verified the same way. `showcase_island_reflection_ab_same_camera.jpg` is different in
kind and is flagged as such: it is the **harness's own** 2×2 sheet, assembled from the four in-memory arm
buffers by the capture code (half-scale nearest-neighbour blits, 8 px gutters, no text, no colour change) and

The three LookDev 001 candidate images (`lookdev_a7_clear_lagoon.png`, `lookdev_a7_technical.png`,
`lookdev_c_stylized_shoreline.png`) are **byte-exact copies** of their development-tree sources
(`Assets/FluidMatter/Showcase/LookDev_001/Screenshots/R5/LD_R5_A7_hero.png`,
`LD_R5_A7_technical.png`, `LD_R5_C_hero.png` respectively), verified by SHA-256 digest match above.
They were captured at 1920×1080, debug mode 0, wave time t=14 s, D3D11 / RTX 4070, Gamma colour space,
pipeline `FMS_W0_URP`. No recolouring, AI upscaling, or post-processing was applied. These are
**LookDev Candidate / Work in Progress** captures — not accepted beauty covers.
encoded as JPEG at quality 92 — the only lossy file published here, chosen because it is a comparison sheet
rather than a pixel-exact claim. Its constituent frames are available losslessly in the development tree, and
its digest is the one the harness recorded.

## Selection method, so the omissions are auditable

### Round 2 — newest portfolio frames (2026-10-16)

Candidates were the 22 images in `Assets/FluidMatter/Showcase/Screenshots/` (17 PNG + 5 JPG). Each was checked
against the capture records in the same directory (`R03_REFLECTION_AB.json`, `B0*_RENDER_HEALTH.json`,
`B0*_CAPTURE_LOG.txt`) rather than against its filename, and required to: exist with the digest its harness
record claims, have `magentaPixels = 0` and `nonFinitePixels = 0`, have a water surface a viewer can judge, and
belong to a systems state whose components are accepted. Three frames passed and are published above; the rest
were rejected with reasons:

- `B02_Final_Underwater.png` / `B0{2,4,5}_Underwater_RAW.png` — **not published at any label.** The underwater
  camera is a failed capability test with status `UNSUPPORTED_CAMERA_VIEW`; publishing it as a portfolio frame
  would claim an underwater system that does not exist.
- `B02_Final_Hero.png` (identical bytes to `B05_Hero_RAW.png`, `a493e9ba…`) — the pre-reflection hero frame. The
  water surface is dominated by a muddy band with nothing to reflect; it is superseded by
  `showcase_island_reflection_on_hero.png` from the same camera and pose.
- `R03_Hero_Reflection_OFF.png`, `R03_Technical_Reflection_OFF.png` — correct, controlled, and black where the
  water should be. Kept as A/B evidence and visible inside the comparison sheet, not published as standalone
  showcase images.
- `R03_Reflection_Debug.png` — debug mode **6 = REFLECTION**, a diagnostic encoding of the sampled vector. It is
  the frame that *proves* the reading is directional (4.77× / 8.01× sky-over-ground error ratio), and its
  numbers are quoted above; the image itself is not appearance, so it stays out of the gallery.
- `B02/B04/B05_ContactSheet.jpg`, `B02_BeforeAfter_ContactSheet.jpg` — multi-panel overviews of the earlier
  shoots whose individual frames are covered by the bullets above; the contact sheets add no view not already
  published or explicitly rejected.
- `B02_Hero_RAW.png`, `B02_Technical_RAW.png`, `B02_Underwater_RAW.png`, `B04_Hero_RAW.png`,
  `B04_Technical_RAW.png`, `B04_Underwater_RAW.png` — per-camera raw buffers of the two earlier shoots, each
  with a distinct digest (verified, not assumed). They pre-date both the reflection binding and the framing
  changes that produced the published poses. `B05_Hero_RAW.png`, `B05_Technical_RAW.png` and
  `B05_Underwater_RAW.png` are byte-identical to the `B02_Final_Hero` / `B02_Final_Technical` /
  `B02_Final_Underwater` composites, which are already handled in this list, so publishing them would duplicate
  pixels under a second name.

The **cover decision was made separately from the selection**: the newest hero frame is the best water in this
project, and it is still not the front image, because the scene around it is a blockout. The front image stays
the accepted W3.1 capture.

### Round 1 — accepted-milestone gallery (2026-10-15)

Candidates were enumerated from `Docs/Evidence/{W2.2,W3.0,W3.1,W4.0}/screenshots/` and
`Docs/Evidence/W3.1/_preview/` — 1,288 PNGs — then filtered by: milestone accepted; debug mode 0 (final
composite); actually shows water; not a byte-duplicate of an already-published image. SHA-256 de-duplication
mattered: 226 distinct 1280×720 W3.1 images collapse from 362 files, and `01_Flat_clear_pool_mode0.png` is
the same bytes as `08b_After_W3_1_consumers_on.png`.

Rejected with reasons, rather than silently dropped:

- `03_Sloped_shore_mode0.png` — accepted capture, but the majority of the frame is one flat unlit colour;
  it would read as a colour field, not water.
- `05_Grazing_reflection_foam_mode0.png` — accepted, but adds nothing over the pool and close-up shots at a
  weaker composition.
- `06_Debug_boundary_mode10.png`, `07_Debug_foam_mode13.png` and ~300 SHF/CAL field frames — diagnostic
  encodings, not appearance.
- All 53 `_preview/` framing-study frames — these are the rejected compositions from the same shoot
  (unlit cubes, black voids, geometry off-frame).
- All 61 `Docs/Evidence/W4.0/screenshots/` frames — **nothing from W4.0 is published, at any label.** They
  are 256–512 px diagnostic dumps over a 3×3 marker grid; none contains a water surface a viewer could
  judge, and W4.0 is not an accepted milestone.

## Attribution corrections made during earlier audits, recorded rather than silently fixed

- **This document previously stated "the project has no art-directed water scene capture yet."** That became
  false when `FMS_Showcase_Island` was rendered. The correction is recorded here instead of the sentence being
  quietly rewritten. Actual state: the art scene **has** been captured in the real engine; one technical frame
  is now published above as work in progress, and the **Hero cover candidate is still withheld** because the
  visual review scored it below the bar for a cover. So "no *published* beauty cover" remains true; "no
  art-directed capture exists" does not.

- **README's front-image caption previously stated "the project's first art-directed scene has not been rendered
  yet."** That became false when `FMS_Showcase_Island` was rendered, and it was still false when the newest
  frames were added. The sentence has been replaced with the actual state (the scene is rendered; its newest
  frames are published as work in progress; the cover is withheld because the non-water art is a blockout). The
  correction is recorded here rather than left as a silent rewrite.

- Likewise, this document's earlier line that the WIP frame's reflection input was "pending an art fix in
  progress" is now superseded: the fix was made, measured with a single-variable A/B, and its two result frames
  are published above. The `B02`/`B05` capture records still describe **their own** frames accurately —
  `_FMS_EnvironmentReflectionAvailable = 0` for the WIP frame is the true read-back of that capture, and has not
  been edited to match the newer state.

- `gerstner-wave-mesh.png` was previously described only as a "wave validation capture". It is in fact
  rendered at debug mode 0 (final composite); the line overlay visible in it comes from the validation rig's
  analytic reference curves, which is exactly why it does not read as water and is not used as a cover.
- `boundary-field.png` was previously attributed to the preview set. Its actual source is the BRD-02 band
  geometry gate.

## What is deliberately absent

- **No published HERO cover.** The art-directed scene has four published frames and one of them
  (`showcase_island_reflection_on_hero.png`) is the best water this project has rendered. It is still not the
  cover: the review scores it below the bar because the scene's non-water art is a blockout — billboard ground,
  a flat background band, placeholder rocks — and a cover promises a finished look the project does not have.
  The bounded art fix that *was* outstanding (the environment reflection input) is now bound and measured, so
  the remaining reason is the environment art, and it is stated as the reason. The front image stays an accepted
  W3.1 technical capture, explicitly labelled as one. This is listed as *not published* in the README and status
  doc instead of being filled with a debug mesh, a diagnostic tint, or a generated image.
- **No underwater imagery.** The scene's third camera looks from below the surface and its frames are kept as
  internal failure evidence with status `UNSUPPORTED_CAMERA_VIEW`. No underwater frame is published, and
  nothing in this repository claims full underwater rendering.
- **No motion demo.** No actual-run clip is published yet.
- **No caustics imagery.** See the W4.0 rejection above.
- **No raw internal logs or gate reports.** The private development record is not mirrored here; this
  repository publishes method, numbers and a small audited image subset.
- **Course material, third-party references and any unverified-rights asset: `NEEDS_RIGHTS_REVIEW` — not
  published.**

## Rights

Rendering context: Tuanjie / Unity URP. Unity, Tuanjie and related third-party technology names belong to
their respective rights holders. This repository does not redistribute engine or package content, and grants
no new licence to third-party material. Public visibility of these documents and images is **not** an
open-source licence for the project or its source.
