# Media attribution & publication scope

Every image in `media/` is an **actual capture from this project's own engine runs** on a controlled
validation rig. No image is AI-generated, no image is a marketing render, and no diagnostic view is
presented as finished appearance. Copies retain the source pixels; where a figure is assembled, that is
stated below.

| File | Source capture | Gate / milestone | Debug mode | Role |
|---|---|---|---|---|
| `current-build-composite.png` | `PREVIEW_03_wall_side_waves2_mode0_composite` | W3.0 preview set | 0 = final composite | What the accepted build actually draws; validation rig, not an art pass |
| `boundary-same-view-three-readings.png` | `PREVIEW_03_wall_side_waves2_mode0_composite` + `_mode10_field` + `_mode11_preview` | W3.0 preview set | 0 / 10 / 11 | Annotated 3-up figure, same camera and same two active waves |
| `boundary-preview.png` | `PREVIEW_01_wall_side_mode11_preview` | W3.0 preview set | 11 = boundary validation preview | Diagnostic tint over composite; **not** shoreline foam |
| `boundary-field.png` | `BRD02_wall_outer0.10000` | W3.0 BRD-02 band geometry | 10 = boundary field encoding | R = proximity, G = validity, B = normalised thickness |
| `gerstner-wave-mesh.png` | `ger02_side_two_final` | W2.1 GER-02 multi-wave superposition | 0 = final composite | Technical evidence; the green/blue polylines are the rig's analytic reference curves |

Integrity hashes (SHA-256, first 16 hex) for the published bytes:

```
current-build-composite.png               1280x720   172417 B   812b9258136600ea
boundary-same-view-three-readings.png     2308x536   126834 B   f5df35192f4a5553
boundary-preview.png                      1280x720    38185 B   c0ce8d332899a72d
boundary-field.png                        1280x720    20951 B   5369aa2de7c5c7ee
gerstner-wave-mesh.png                     960x540   127474 B   b1229eebb60f68dd
```

`current-build-composite.png`, `boundary-preview.png`, `boundary-field.png` and `gerstner-wave-mesh.png`
are byte-identical copies of the named captures. `boundary-same-view-three-readings.png` is the only
composed figure: three unmodified captures of the same camera, each down-scaled with Lanczos and given a
text label, on a neutral canvas. No pixels were painted, retouched, or substituted, and the caption on the
figure states that panels B and C are diagnostic views.

Two attribution corrections were made during this audit, and are recorded rather than silently fixed:

- `gerstner-wave-mesh.png` was previously described only as a "wave validation capture". It is in fact
  rendered at debug mode 0 (final composite); the line overlay visible in it comes from the validation rig's
  analytic reference curves, which is exactly why it does not read as water and is not used as a cover.
- `boundary-field.png` was previously attributed to the preview set. Its actual source is the BRD-02 band
  geometry gate.

## What is deliberately absent

- **No HERO beauty pass.** The project has no art-directed water scene capture yet; this is listed as
  `NEEDS_CAPTURE` in the README and status doc instead of being filled with a debug mesh, a diagnostic tint,
  or a generated image.
- **No motion demo.** No actual-run clip is published yet.
- **No raw internal logs or gate reports.** The private development record is not mirrored here; this
  repository publishes method, numbers and a small audited image subset.
- **Course material, third-party references and any unverified-rights asset: `NEEDS_RIGHTS_REVIEW` — not
  published.**

## Rights

Rendering context: Tuanjie / Unity URP. Unity, Tuanjie and related third-party technology names belong to
their respective rights holders. This repository does not redistribute engine or package content, and grants
no new licence to third-party material. Public visibility of these documents and images is **not** an
open-source licence for the project or its source.
