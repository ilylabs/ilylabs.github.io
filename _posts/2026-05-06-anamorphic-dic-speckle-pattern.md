---
layout: single
title: "Anamorphic Speckle Patterns: Fixing DIC for Large Structures"
date: 2026-05-06
author: "Stéphane W. Hu, Guilhem Marchal, Yvan Dilem, Denis Walch, Ilyass Tabiai"
categories: [Experimental Mechanics, Aerospace]
tags: [DIC, Digital Image Correlation, Speckle Pattern, Anamorphic Transformation, Aerospace Testing, Optics]
excerpt: "Measuring large aircraft structures with Digital Image Correlation from tilted camera angles creates perspective distortions that degrade measurement quality. New research shows how anamorphic transformation of speckle patterns can pre-compensate for these effects, improving measurement precision."
---

Published in [*Optics and Lasers in Engineering* (2026)](https://doi.org/10.1016/j.optlaseng.2026.109773).
{: .notice--info}

## The Problem with DIC on Large Structures

Digital Image Correlation (DIC) is a standard tool for measuring deformations in experimental mechanics. It works by tracking a random speckle pattern on a surface between images. But when you're measuring large structures like aircraft wings or fuselages, you often can't position the camera perpendicular to the surface.

This creates a real problem: **perspective distortions cause the speckles to appear non-uniform in the camera view**, degrading correlation quality and increasing measurement uncertainty.

<div style="text-align: center;">
  <img src="/assets/img/anamorphic-dic/fig003.png" width="80%" alt="Figure 2: Comparison between speckle patterns with and without perspective effects. Left: circular speckles viewed perpendicular to camera. Right: same speckles viewed from tilted angle — they appear deformed and non-uniform in size." />
  <p><em>Figure 2: Perspective distortion effect on speckle patterns. Left: circular speckles viewed perpendicular to camera. Right: same speckles viewed from tilted angle — they appear deformed and non-uniform in size.</em></p>
</div>

### The Anamorphic Solution

The key insight is simple: **if you know the camera geometry, you can pre-distort the speckle pattern so it appears uniform to the camera**.

Think of it like anamorphic art — the image looks stretched when viewed from certain angles, but appears normal from a specific viewpoint. Here, the "anamorphic" speckle pattern is designed to compensate for the camera's perspective, so the DIC algorithm sees uniform speckles.

<div style="text-align: center;">
  <img src="/assets/img/anamorphic-dic/fig004.png" width="80%" alt="Figure 3: Anamorphic transformation of a circular speckle projected onto a developable surface. Shows how different points on the surface project to the same location on the camera sensor due to the anamorphic transformation." />
  <p><em>Figure 3: Anamorphic transformation concept. Shows how a circular speckle is geometrically transformed so it appears uniform from the camera's viewpoint, even when projected onto a tilted surface.</em></p>
</div>

{: style="text-align: justify;"}

### How It Works

1. **Geometric model** — Known camera position and surface geometry define the perspective transformation
2. **Pattern generation** — A reference speckle pattern is locally transformed according to this transformation
3. **Physical application** — The anamorphic pattern is printed and applied to the test surface
4. **Standard DIC** — No changes to the correlation algorithm; it sees uniform speckles

{: style="text-align: justify;"}

### Experimental Validation

The team tested this on a planar target with controlled tilt angles up to 89.7° — extreme grazing angles representative of real aircraft testing scenarios.

<div style="text-align: center;">
  <img src="/assets/img/anamorphic-dic/fig005.png" width="70%" alt="Figure 4: Experimental setup scheme. Two planes tilted at angle θ, with cameras at fixed distance. Regular speckle pattern (SP-Ref) on reference plane, regular (SP-1) and anamorphic (SP-2) patterns on tilted plane." />
  <p><em>Figure 4: Experimental setup diagram. Reference plane with regular speckle (SP-Ref) at 2.5m from cameras. Tilted plane with both regular (SP-1) and anamorphic (SP-2) patterns for comparison.</em></p>
</div>

<div style="text-align: center;">
  <img src="/assets/img/anamorphic-dic/fig006.png" width="80%" alt="Figure 5: Physical experimental setup. Wooden boards on adjustable tripods with printed speckle patterns, stereo cameras, and LED lighting panel." />
  <p><em>Figure 5: Physical setup. Wooden boards on variable-height tripods allow controlled tilt angles. Stereo cameras mounted on fixed support, large LED panel provides uniform illumination.</em></p>
</div>

**Key results:**

<div style="text-align: center;">
  <img src="/assets/img/anamorphic-dic/fig009.png" width="80%" alt="Figure 8: Evolution of matching uncertainty (σ_pixel) across all tilt angles. Blue contours: regular pattern SP-1. Red contours: anamorphic pattern SP-2. Graph below shows mean uncertainty — SP-2 consistently lower than SP-1." />
  <p><em>Figure 8: Main result — matching uncertainty across all tilt angles. The anamorphic pattern (SP-2, red) shows consistently lower uncertainty than the regular pattern (SP-1, blue). Mean values (bottom graph) confirm SP-2 outperforms SP-1 across all angles.</em></p>
</div>

- **Consistent uncertainty reduction** — The anamorphic pattern consistently reduced matching uncertainty compared to regular patterns
- **Improved 3D localization** — Better precision in determining 3D positions of surface points
- **Robust to angle changes** — Benefits maintained even as camera incidence angles increased

{: style="text-align: justify;"}

### Why This Matters

Aircraft testing often requires measuring large structures from fixed positions that create poor viewing angles. Traditional DIC setups struggle with these configurations. The anamorphic approach offers a practical solution:

<div style="text-align: center;">
  <img src="/assets/img/anamorphic-dic/fig012.png" width="80%" alt="Figure 11: 3D point cloud visualization. Blue points: regular pattern SP-1. Red points: anamorphic pattern SP-2. Gray points: reference SP-Ref. Section views show SP-2 points closer to best-fit plane with smaller deviations." />
  <p><em>Figure 11: 3D point cloud comparison. The anamorphic pattern (SP-2, red) produces points that cluster more tightly around the best-fit plane compared to the regular pattern (SP-1, blue), demonstrating improved 3D localization precision.</em></p>
</div>

- **No camera repositioning needed** — The pattern compensates for the camera angle
- **Works with existing DIC equipment** — No special cameras or algorithms required
- **Extends measurement range** — Enables testing from positions that were previously too distorted for reliable DIC

{: style="text-align: justify;"}

### Next Steps

While this study validates the approach on planar surfaces, the method is designed to extend to complex curved surfaces (aircraft wings, fuselages) and full-scale structures. The transformation framework is generic and can be applied offline during pattern generation.

{: style="text-align: justify;"}

---

**The paper:** "Planar validation of speckle pattern optimization for large-scale 3D streamlined structures in stereo digital image correlation using anamorphic transformation" by Stéphane W. Hu, Guilhem Marchal, Yvan Dilem, Denis Walch, Ilyass Tabiai. *Optics and Lasers in Engineering*, 202(2026) 109773. [DOI](https://doi.org/10.1016/j.optlaseng.2026.109773).
