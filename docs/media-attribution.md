# Media attribution & publication scope

Every image in `media/` is an **actual capture from this project's own engine runs**, either on a controlled
validation rig or, for the one development-showcase frame below, in the project's portfolio scene. No image is
AI-generated, no image is a marketing render, and no diagnostic view is presented as finished appearance.
Copies retain the source pixels; where a figure is assembled, that is stated below.

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
the publication bar, so it is withheld pending an art fix (see *What is deliberately absent*).

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

Full value for the development-showcase frame, since it is the one published image whose claim depends on
being the engine's own bytes:
`66fd061306b5ad97eba3b6e0da469363df08e5cf1511bc57bfa49d268505d5f9` (920,939 bytes). The same digest is recorded
by the capture harness inside `B05_RENDER_HEALTH.json` under `frame_B05_Technical_RAW.png_sha256`, so the
published file, `B02_Final_Technical.png` and the RAW engine output are one and the same byte stream.

The four W3.1 single-panel files are **byte-exact copies** of their named sources — not recompressed, so no
lossy or generative alteration is even possible. `current-build-composite.png`, `boundary-preview.png`,
`boundary-field.png` and `gerstner-wave-mesh.png` are likewise byte-identical copies. The two assembled
figures (`w31_consumers_off_on_same_camera.png`, `boundary-same-view-three-readings.png`) are unmodified
panels, Lanczos-resized and labelled, on a neutral canvas. No pixels were painted, retouched, or
substituted in any image here, and every assembled figure says so in its caption. `showcase_island_technical_wip.png`
is also a byte-exact copy, made with `cp` and confirmed by `cmp` against its source in the development tree.

## Selection method, so the omissions are auditable

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

- `gerstner-wave-mesh.png` was previously described only as a "wave validation capture". It is in fact
  rendered at debug mode 0 (final composite); the line overlay visible in it comes from the validation rig's
  analytic reference curves, which is exactly why it does not read as water and is not used as a cover.
- `boundary-field.png` was previously attributed to the preview set. Its actual source is the BRD-02 band
  geometry gate.

## What is deliberately absent

- **No published HERO cover.** The art-directed scene has real captures, and one of them is published above as
  a development showcase, but the Hero frame is **held back**: the review scored it below the bar for a cover
  and one bounded art fix (the environment reflection input) is in progress. The front image stays an accepted
  W3.1 technical capture, explicitly labelled as one. This is listed as *not published* in the README and
  status doc instead of being filled with a debug mesh, a diagnostic tint, or a generated image.
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
