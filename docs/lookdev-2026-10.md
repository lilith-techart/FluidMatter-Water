# October 2026 — FluidMatter Water LookDev 001

**Status:** Real Tuanjie URP captures and controlled research. **Not** a new rendering milestone, final beauty cover, GPU benchmark or an accepted caustics result.

## Why this research matters

The previous water was identifiable but too flat. Instead of replacing the rendering system, we compared water absorption, shallow/deep colour, two surface-normal layers, refraction, shoreline and foam in a fixed scene. The study used **12 controlled arms**, **74 real engine PNG captures**, and **6 contact sheets** at **1920 × 1080**, fixed cameras and wave time (t = 14), on D3D11 / RTX 4070.

The most recent raw A7/C image files remain in the private development workspace, pending a separate, hash-verified binary upload. **This public document does not pretend they are hosted here.**

## Real measured LookDev results

| Arm | Change or visual role | Measured result |
| --- | --- | --- |
| A0 | Authored baseline | Technical water-mask B−R = **−0.0716** |
| A1 | Beer–Lambert absorption sigma RGB **(0.35, 0.72, 1.45) → (0.46, 0.21, 0.09)** | Technical B−R = **+0.2504** |
| A2 | Add cyan body tint **(0.05, 0.28, 0.30)** | Hero water-mask B−R = **+0.2570** |
| A6 | Enable both existing normal layers | Hero mean local luminance gradient **0.00295 → 0.00634** vs A2 |
| **A7** | **Clear Lagoon**, final tuned candidate | Technical B−R = **+0.2617**; preferred realistic art direction |
| B | Luminous Water | Brightest thumbnail, but little micro-surface structure |
| C | Stylized Shoreline | More visible foam; useful stylized alternative |
| A0b | Baseline inertness control | Byte-identical to A0 |

The B−R and gradient measurements sample **water pixels only**, not whole-screen averages. They measure colour/contrast changes; they are **not** physical-validation proofs or objective beauty scores. The internal rubric gave A7 58.75/100 and C 59.25/100. These nearly tied subjective scores are **not** research PASS/FAIL gates.

## Rendering TA technical strengths

- One positive linear eye-depth/thickness convention feeds per-channel Beer–Lambert absorption.
- Bounded screen-space refraction with invalid foreground rejection.
- Fresnel + environment cubemap **preview**, not SSR or planar reflections.
- Gerstner displacement with distinct geometry and shading normals.
- Depth-derived shoreline and adjustable foam coverage.
- ScriptableObject Profile → Binder → shader path; isolated engine-frame comparisons with control arms and retained failures.

## Important corrections and missing pieces

**The older R03 Reflection_ON hero and technical screenshots are invalid as finished water-appearance evidence.** A later R8 test found FresnelF0 = 1 and ReflectionStrength = 1 retained on a runtime material clone, so the actual transmitted colour term vanished. R03 media can demonstrate cubemap binding, but not a correctly composed transparent water surface. The original R03 image bytes should be retained for provenance, **not** featured as a beauty shot.

The temporary procedural normal assets visibly repeat as cross-hatch/dots at grazing angles despite passing tileability checks; a proper licensed natural water-normal asset is still required. The current foam threshold controls area, not natural foam-sheet morphology. A directional sun-glint/specular lobe (E1) is **design only** and is not implemented. The underwater camera does not prove underwater rendering.

**Caustics W4.0 remains BLOCKED.** Frozen CAU-14: **261 graded rows, 241 PASS, 20 FAIL**. None of the LookDev 001 captures enables caustics, and no artwork here claims flux conservation or a validated physically accurate caustics model.

**GPU time remains unknown.** The existing headless camera.Render/ReadPixels harness cannot resolve shader deltas above its timing noise. Do not reuse these data as FPS or GPU milliseconds.

## Provenance for the next image publication

Original captures to upload byte-exactly from the owner-approved local handoff:

- A7 hero: `R5/LD_R5_A7_hero.png`, SHA-256 `275cbfd13673c3d9c4849216061289e7995c6a8ea6b95ab922602bd0eb9177ad`
- A7 technical: `R5/LD_R5_A7_technical.png`, SHA-256 `2e74cd7143a6342f24aa9027bb395895a6a7e888f62207da35335eeb3a8d668b`
- C hero: `R5/LD_R5_C_hero.png`, SHA-256 `cc8f4eae9b29e485d9656b8d93561e05b85396652d1f954612274825d7348280`

**Caption guardrail:** R8 debug modes M2 (thickness) and M3 (transmittance) belong to the **A0 baseline**, not A7. A0's transmittance order is R > G > B; A7's ordered sigma implies the opposite ordering at equal positive thickness.

The public repository is a **curated technical-art showcase**, not a full Unity source release. The normal assets, performance claims, pending E1 implementation, caustics and underwater capabilities must not be overstated.
