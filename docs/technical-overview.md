# FluidMatter — Technical Overview

Deeper detail than the README. Every statement here is traceable to the accepted development record
or to the shader/contract source it describes.

## Design rule: one owner per quantity

Each quantity in the pipeline has exactly one defining module, one validity rule, and an explicit list
of consumers. A consumer may read a value; it may not redefine it. The recurring failure mode this
prevents is a downstream effect inventing its own interpretation of "how deep is the water here" and
then disagreeing with absorption at the contact edge.

The rule has teeth: the boundary milestone explicitly refused to publish curvature, shore direction,
scene normal, world-space distance or anything temporal, because a second Euclidean "distance to
shore" would let two numbers claim to be the same quantity.

## Depth contract

A single **positive linear eye depth** convention. Water thickness is valid scene depth minus surface
depth, clamped non-negative, and it is measured **along the view axis** — an eye-depth thickness, not
the shortest Euclidean distance to a shoreline. That distinction is documented rather than glossed,
because a future shoreline look must reason about the difference (measured band-width variance across a
30° view ramp is ~4.98×, which is the size of the error a naive reading would carry).

Validity is a real flag, not an inference from magnitude. Sky, far-plane and foreground-rejected samples
leave `valid = 0` and contribute nothing to any downstream field; the field is never extrapolated past
the visible water silhouette.

## SurfaceData contract

The shared surface struct published to all downstream shading:

| Field | Meaning |
|---|---|
| `positionWS` | Rendered world-space surface position (base position + wave displacement) |
| `displacementWS` | The Gerstner displacement itself, so `positionWS - displacementWS` recovers the material point |
| `geometryNormalWS` | Unit analytic surface normal |
| `tangentWS` / `binormalWS` | Parametrisation partials dP/du, dP/dv of the water-plane map — surface partials, **not** mesh UV tangents |
| shaded normal | The unit normal handed to lighting/optics, filled by the shading-normal step; equal to `geometryNormalWS` before that call |

Geometry normal and shaded normal are deliberately separate members. Consumers name which one they use.

## Waves

Gerstner displacement, up to four authored slots. Per wave: direction (world XZ), amplitude, wavelength,
speed, steepness, with `k = 2π/L` and phase `k·(D·xz) − k·speed·time`. Horizontal amplitude is `S·A`, so
the model states its own crest-folding condition. Degenerate and non-finite parameters (amplitude ≤ 0,
wavelength below a floor, non-finite speed/steepness) are rejected at evaluation instead of propagating.

Validation compares GPU output against a CPU reference implementation of the same maths, including the
analytic derivative frame against central differences, and includes a counterfactual that proves the
"add the normals" composition bug is absent rather than merely unobserved.

## Optics

- **Refraction** is screen-space: offset derived from the surface normal, bounded by a maximum offset in
  pixels, with a viewport-edge fallback to the original UV and explicit rejection of foreground geometry.
  It can only move information already present in the scene colour/depth buffers.
- **Transmittance** is Beer–Lambert with per-channel absorption coefficients and a tint term, monotonic
  non-increasing in thickness per channel, with zero thickness reducing to no attenuation.
- **Reflection** is a Fresnel-weighted (Schlick) composition against a controlled environment probe. It is
  a **preview** path. It is not screen-space reflection and not planar reflection, and the availability of
  environment content is gated by an explicit flag so the shader never consumes undefined probe data.

## Boundary field (accepted as data)

```
FMS_BoundaryData {
    float thicknessEye;  // view-axis water thickness to the opaque scene behind the displaced surface; 0 when invalid
    float proximity;     // 1 at contact, 0 at/beyond the outer authoring distance, affine between, 0 when the sample is invalid
    float valid;         // trustworthiness of the depth sample — NOT a feature-enable flag
}
```

Evaluation consumes the interpolated `SurfaceData.positionWS`, i.e. the displaced surface, so the field
tracks moving waves instead of the flat plane. A zero-width band legitimately leaves `valid = 1` with
`proximity = 0`; a sky or foreground-rejected pixel leaves `valid = 0`.

**No production look consumes the boundary at the accepted state.** The only consumers are a debug readout
and a validation preview, and accepted profiles author the feature off (`_boundaryEnabled: 0`).

## Profiles & binding

Authored profiles carry appearance parameters only; debug, camera, benchmark and capture state live
separately. Two binding backends are maintained — a shared material cache keyed by material identity, and
per-renderer `MaterialPropertyBlock` writes — and they are proven to produce identical pixels rather than
assumed equivalent. Hostile payloads (non-finite values, negative distances, inverted bands) are sanitised
at the boundary of the system, with the sanitisation itself under test.

## Debug view contract

Mode numbering is a frozen contract; adding a view never renumbers an existing one.

| Mode | View | Nature |
|---|---|---|
| 0 | Final composite | production output |
| 1 | Scene depth | diagnostic |
| 2 | Water depth / thickness | diagnostic |
| 3 | Absorption / transmittance | diagnostic |
| 4 | Refraction offset & validity | diagnostic |
| 5 | Surface normal | diagnostic |
| 6 | Reflection weight & colour | diagnostic (preview path) |
| 7 | Wave displacement | diagnostic |
| 8 / 9 | Wave tangent / binormal | diagnostic |
| 10 | Boundary field encoding (R proximity, G valid, B thickness) | diagnostic |
| 11 | Boundary validation preview (tint over composite) | diagnostic — **not** a shoreline effect |

Out-of-range mode values resolve safely, and one reserved mode renders a documented neutral value so that
"no feature" is distinguishable from "feature broken".

## Module boundaries

Core shader modules own generic maths and data contracts; the URP-facing layer reaches engine services only
through a pipeline adapter; validation automation and scenes never leak into the runtime path. Ownership and
include-direction rules are checked statically as part of the gates rather than enforced by convention.
