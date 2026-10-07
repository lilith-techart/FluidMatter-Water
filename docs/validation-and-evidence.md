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
  record still shows fail → diagnose → fix → pass. Nine such archives exist for the boundary milestone.
- **Verdict integrity guards.** The harness deliberately injects a graded disagreement to prove its own verdict
  guard fires, and logs it, rather than assuming the guard works.
- **Line-ending discipline.** The project sets `core.autocrlf=true`, which makes a naive dirty-tree check
  over-report after headless runs; drift is classified by content hash, and restores are done by explicit path
  only. This is a real hazard that was documented after it cost work, not a theoretical note.

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

## Public vs private record

The full development record — gate reports, automation logs, per-run CSVs, diagnostic archives — stays in the
private project. This repository publishes the method, the numbers that support each claim, and a small audited
subset of captures. Raw internal logs are not mirrored here.
