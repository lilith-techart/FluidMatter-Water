# FluidMatter — Validation & Evidence

How claims in this repository are produced, and where the method stops being useful.

## Method

Each milestone is gated by an automated in-editor harness that runs headless, renders controlled validation
rigs with known geometry and known camera poses, and compares the GPU result against a **CPU reference
implementation of the same mathematics**. Every graded row is written to an authoritative CSV with its input
configuration, measured value, expectation and verdict — so a pass can be re-derived from the numbers rather
than trusted as a sentence.

Supporting practices that are part of the method, not decoration:

- **Isolation.** Gates run against a snapshot-restored asset tree; a gate that leaves project assets modified
  is not considered clean. The accepted state reports `isolation_unresolved = 0`.
- **Preserved failure.** Failing runs are archived with their FAIL rows instead of being overwritten, so the
  record still shows fail → diagnose → fix → pass. Nine such archives exist for the boundary milestone and
  six for the shoreline/foam milestone.
- **Verdict integrity guards.** The harness deliberately injects a graded disagreement to prove its own verdict
  guard fires, and logs it, rather than assuming the guard works.
- **Line-ending discipline.** The project sets `core.autocrlf=true`, which makes a naive dirty-tree check
  over-report after headless runs; drift is classified by content hash, and restores are done by explicit path
  only. This is a real hazard that was documented after it cost work, not a theoretical note.

## Accepted W3.1 shoreline & foam evidence summary (current head of the chain)

| Property | Value |
|---|---|
| Graded rows in the authoritative gate table | 342, `result = PASS` on every row |
| Gates covered | HARNESS + SHF-01…SHF-12 + CAL + BEAUTY + PERF |
| Regression | 16 gates × 2 independent passes (sequential + isolated), all `PASS`, exit 0 |
| Isolation | 265 project files snapshotted per gate; `isolation_drifted = 36 == isolation_restored = 36`; `isolation_unresolved = 0`; `files_created_by_gate = 0` |
| Compile / shader / IO health | `errorCS = 0`, `shaderError = 0`, `ioError = 0` on every row of both passes |
| Archived failing runs | 6, retained with their FAIL rows |

Representative graded results, with their scope:

- **Disabled identity** — `ShorelineEnabled = 0` reproduces the W3.0 render: max channel difference
  `0.00000`, differing pixels `0`, alpha differing pixels `0`, over `469,236` owned pixels, with `19,652`
  band pixels present so the comparison is not between two empty frames.
- **Shaping** — the shipped curve is flat at both band endpoints (slope `0.00300` near zero, `0.00298` near
  one, per unit proximity); a 1001-point analytic sweep shows 0 monotonic violations, and the rendered mode-12
  mask equals the shape of the same pixel's proximity to `0.00033` against a capture floor of `0.00195`.
- **Areal honesty of coverage** — over a 3×5 softness × coverage grid, measured mean breakup matches the
  analytic ramp to ≤`3.0e-05`, and the measured non-zero area fraction matches authored coverage to ≤`0.001`.
- **Authoring cost (CPU-side, explicitly not a GPU claim)** — the graded row
  `consumersMinusControlTickMs = 0.05606` ms per authored tick, published with its own caveat: the whole
  payload is rebuilt on every authored tick, so this is rebuild granularity rather than a per-parameter
  price, and no threshold is asserted around it. The milestone report separately records binder cost at
  `0.00557 ms/tick` unchanged vs a `0.00519 ms/tick` pre-W3.1 control.

**A published limitation, not hidden:** authored coverage is a *field-level* calibration. The SHF-05 band
measures `breakupAreaFraction = 0.47995` against a predicted `0.60000`, published as its own rows
(`breakupAreaPredictionErr = 0.12005`) with no acceptance threshold attached, and its mechanism was
deliberately not asserted. One should not quote coverage as a promise about the foam-pixel count of any
particular band.

## Accepted W3.0 evidence summary

| Property | Value |
|---|---|
| Gates, per pass | 15 (harness, BRD-01…BRD-10, precision, view angle, preview, performance) |
| Independent passes | 2 (sequential + isolated) |
| Graded measurement rows | 3,343 |
| FAIL rows in authoritative CSVs | 0 |
| `isolation_unresolved` | 0 |
| Archived failing runs | 9, retained with their FAIL rows |

Representative graded results, quoted with their scope:

- **Binding parity** — shared material cache vs `MaterialPropertyBlock`: `469,236` pixels compared,
  `parityMaxChannelDiff = 0`, `parityDisagreeingPixels = 0`, identical band occupancy.
- **Occlusion honesty** — `227,624` occluded footprint pixels measured, `0` painted through,
  foreground prediction agreement `1.0` against a ≥0.99 bar.
- **Temporal stability** — 600 frames on a two-wave displaced surface: `nanFrames = 0`, `infFrames = 0`,
  `nonFinitePixelsTotal = 0`, solver residual `≤ 1e-5 m`, no unexplained frame-to-frame delta.
- **Precision floor** — smallest stable boundary distance `0.001 m`; half-float `encodingStepM = 0.00024 m`
  **at the 0.50 m ruler**. This is a storage floor at one specific ruler, not a resolution claim, and it does
  not transfer to another resolution without re-measuring.
- **Sanitisation** — six hostile payload sets all yield `payloadNonFiniteEdges = 0` and
  `renderedProximityMax = 0`, with zero thickness-channel drift across `460,800` pixels.
- **Wave coupling** — boundary band area and centroid motion tracked across the displaced surface
  (`centroidMotionFrames = 599/599`), with an all-zero static control.

## What the method cannot currently show

Stating these as gaps, because "we did not measure it" is a different claim from "it is fine":

1. **No GPU cost.** The headless capture path (render-texture allocation, two explicit renders, readback and a
   large managed pixel copy) measures as a substantial fraction of the frame — reported across the run as
   0.94–1.08 of the bracket mean, and above 1.0 in some legs, meaning the harness costs as much as or more
   than the frame it captures. No enabled/disabled shader delta was therefore resolvable above the harness's
   own noise. Frame-time figures exist and are published, but no threshold is asserted around them and none
   should be read as a GPU benchmark.
2. **No shader throughput counters.** Compiler-reported texture-sample and interpolator statistics were
   unreliable in this editor version, so shape claims are declaration/CB/keyword counts, not hardware cost.
3. **Narrow perceptual scope.** View-angle conclusions come from one 30° ramp, one horizontal shoreline, one
   resolution (1280×720) and one arc distance. Rejection of an alternative proximity reading is argued from
   measured texel scale — a physical-resolution argument, not a perception study.
4. **Flat-water split.** Band-geometry gates and the view-angle study run with waves off; the displaced-surface
   legs are separate gates. The two are not conflated into one number.
5. **Single-slot water renderer.** Multi-slot and batching variations were not exercised for the boundary path.
6. **Some early-run detail is unrecoverable.** A subset of rows and screenshots from a first failing run was
   overwritten before the incident was detected; only the verdict line and the FAIL rows carry forward. The
   rule added in response was to copy each run's artefacts to a timestamped directory before a later run can
   touch them.

## The showcase reflection A/B — what kind of evidence it is

The three newest published images come from a capture harness, **not** from the accepted gate chain, and the
distinction is not cosmetic. Gate chains grade controlled rigs against a CPU reference of the same maths; this
run grades one visual claim, in one scene, at 1920×1080. Its status label in the README is therefore
**Art pass / Work in Progress**, not *accepted*.

What was fixed before the run, in the harness itself, rather than chosen after seeing the pictures:

- **One variable.** The two arms may differ in exactly one shader property, and the harness reports which one
  (`stateDiff_propertyCount = 1`, `stateDiff_keys = "_FMS_EnvironmentReflectionAvailable"`). A diff of more
  than one property fails the run rather than being narrated around.
- **The difference must be on the water.** Differing pixels outside the water mask must be ≤ 0.02 % of the frame
  and differing pixels inside must be ≥ 99 % of all differences. Measured: `0` outside, share `1`, on both
  cameras, over `929698` and `993534` water pixels.
- **The reading must be directional, not global brightness.** A separate debug-mode-6 arm encodes the sampled
  reflection vector; the criterion is that its error against the *sky* direction be at least 2× better than
  against the *ground* direction. Measured ratio: `4.769322` (hero), `8.012977` (technical).
- **Subsurface must survive.** Chromatic water pixels above the cubemap's own stored channel maxima
  (`0.803922, 0.74902, 0.737255`) must be non-zero in the ON arm and, as a control, zero in the OFF arm.
  Measured: `3188` / `4989` ON, `0` / `0` OFF.
- **Nothing else may move.** Foam highlights must not shrink (measured `19.1×` and `11.7×` ON/OFF), caustics must
  stay disabled in both arms, no magenta and no non-finite pixels, `nonBlackRatio = 1` on the published arms.
- **The project must be untouched.** The scene file hashes identical before and after
  (`e0990dacb0d271f0…`), and three re-hash manifests — 56 protected files, 22 accepted profiles, 44 prior
  screenshots — report `0` mismatches. The harness never marks or saves the scene.

Two consequences of that framing are published rather than hidden. The `OFF` arm renders black water, because a
preview path with no environment input has nothing to reflect — there is no screen-space or planar fallback to
soften it. And the conclusions attach to this scene, these two cameras and 1920×1080: they do not extend the
W3.1 rig's view-angle or resolution envelope, and they do not make the caustics milestone any closer to
acceptance.

## Public vs private record

The full development record — gate reports, automation logs, per-run CSVs, diagnostic archives — stays in the
private project. This repository publishes the method, the numbers that support each claim, and a small audited
subset of captures. Raw internal logs are not mirrored here.
