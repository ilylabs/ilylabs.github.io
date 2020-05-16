---
title: "muDIC"
excerpt: "Full field measurement methods: Digital Image Correlation"
sidebar:
  - nav: "projects-mudic"
header:
  teaser: /assets/images/projects/ComplexMats/muDIC-pointcloud.gif
category: [Heterogeneous materials]
tags: [full field measurements, digital image correlation, open source, open science, python]
partners: PolymerGuy, Nicolas Thuard, d-lamin8, Dr. Patrick Diehl
project_date: "Summer 2020"
contribute: https://github.com/PolymerGuy/muDIC/issues
learn_more: https://github.com/PolymerGuy/muDIC
shared_library:
recruiting:
last_modified_at: 2020-05-14T12:17:43-04:00
usemathjax: true 
---

## Digital Image Correlation

**Digital Image Correlation** (DIC) techniques aim at **measuring the displacement** of a surface out of 2D or 3D[^A] consecutive images of a **deforming surface**. DIC methods rely on image tracking and registration methods to track the movement of various areas of the image. The following example, from the [muDIC documentation](https://mudic.readthedocs.io/en/latest/intro/dic_intro.html), helps understand the basic idea of DIC. 
{: style="text-align: justify;"}

Suppose we have a white surface that we cover with tiny crosses like this one <span style="color:blue">+</span>. Once the surface deforms, if we capture images of the surface at regular intervals while it is deforming, the movement of the <span style="color:blue">+</span> crosses can help us understand and even **quantify the displacement** at various places of our suface. This example is presented in the following figure:
{: style="text-align: justify;"}

<figure class="half">
    <a href="https://mudic.readthedocs.io/en/latest/intro/dic_intro.html"><img src="/assets/images/projects/ComplexMats/muDIC-pointcloud0.png" width="60%"></a>
    <a href="https://mudic.readthedocs.io/en/latest/intro/dic_intro.html"><img src="/assets/images/projects/ComplexMats/muDIC-pointcloud.gif"  width="60%"></a>
</figure>
*The initial square surface covered with <span style="color:blue">+</span> crosses is shown on the left. An animation of the deforming surface is shown on the right (from [^5] ).*
{: style="text-align: center;"}

DIC thus requires:
1. A surface that deforms (well it can **not** deform but...)
2. A **camera**, to capture consecutive images of the surface that is deforming
3. A kind of **pattern** (similar to the <span style="color:blue">+</span> crosses) that would be used to track local motion

The pattern is called a **speckle pattern** as it is often made using a **random arrangement of paint dots**. The speckle pattern is sometimes made using a can of spray paint, an airbrush, stamps and, in some cases, the surface of the material itself has enough *randomness/roughness* to be used as a speckle pattern. The following figure shows two examples of real deforming speckle patterns:
{: style="text-align: justify;"}

<figure class="half">
    <a href="https://mudic.readthedocs.io/en/latest/intro/dic_intro.html"><img src="/assets/images/projects/ComplexMats/muDIC-deformation.gif" width="60%"></a>
    <a href="https://mudic.readthedocs.io/en/latest/intro/dic_intro.html"><img src="/assets/images/projects/ComplexMats/muDIC-speckle048.gif"  width="60%"></a>
</figure>
*Left: Mineral filled PVC under monotonic load[^8]. Right:  Interfacial debonding of a 1mm in diameter PTFE fiber embedded in Epoxy, tensile load is applied along the vertical direction [^7]*
{: style="text-align: center;"}

After covering a specimen with a **speckle pattern**, the specimen is setup in some kind of **testing equipment** (such as a tensile testing machine). Cameras are setup in front of the machine in order to capture consecutive images of the surface that was covered with a speckle pattern. It is usually necessary to undergo some sort of **spatial calibration** here in order to later be able to perform **accurate metric measurements** on the captured images. The following video presents the main steps of a real DIC experiment:
{: style="text-align: justify;"}

<div style="left: 0; width: 100%; height: 0; position: relative; padding-bottom: 62.5%;">
    <iframe src="https://streamable.com/e/uc8pol" style="border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;" allowfullscreen scrolling="no" allow="encrypted-media"></iframe>
</div>

After **analyzing an experiment**, it is possible to **explore full field results in a variety of ways**. Since the complete displacement and strain fields are known, it is possible to inspect various areas using **virtual extensometers** or **local averages**. 
{: style="text-align: justify;"}

## Heterogeneous materials

DIC is an interesting technique as it is an easy way to **gather information about the local behavior** (displacement field) of a material. Nowadays, most methods available in labs to measure strains or displacements are designed to either measure the local strain in a single point (strain gauge) or to measure the global strain over a complete specimen (extensometers). DIC provides a way to measure the local behavior of a material as if the material were completely covered with strain gauges. Such fine information is **not mandatory in the case of homogeneous materials** (such as metallic materials) since it is fairly possible to, given a certain loading and specimen geometry, assume that the material is undergoing continuous (often even linear) deformation. 
{: style="text-align: justify;"}

<figure class="half">
    <a href="https://figshare.com/articles/CT-PMMA-x5_mp4/7766729"><img src="/assets/images/projects/ComplexMats/muDIC-CTcrack.gif"></a>
    <a href="https://link.springer.com/article/10.1007/s11340-018-0429-9"><img src="/assets/images/projects/ComplexMats/muDIC-Spec048-eyy.gif"></a>
</figure>
*Left: Stable crack growth within a CT specimen laser cut out of a PMMA sheet, the vertical strain field was measured using Digital Image Correlation, the applied load is also along the vertical direction [^4] . Right: Interfacial debonding of a 1mm in diameter PTFE fiber embedded in Epoxy, tensile load is applied along the vertical direction[^7].*
{: style="text-align: center;"}

This assumption does not hold in the case of **heterogeneous materials** (such as composite materials) as the material itself is full of **discontinuities** (cracks, interfaces). DIC is thus becoming a critical tool when it comes to better understanding and **characterizing the behavior of heterogeneous materials**. 
{: style="text-align: justify;"}


## $$\mu$$DIC

[$$\mu$$DIC](https://github.com/PolymerGuy/muDIC) is a python toolktit for DIC written in Python. It contains tools designed to perform DIC analysis on experimental data as well as tools to perform virtual DIC experiments. The package was developed by:
* Sindre Olufsen - Implementation - [PolymerGuy](https://github.com/polymerguy)
* Marius Endre Andersen - Wrote the Matlab code on which this is based
Our group will help implement additional features of the $$\mu$$DIC package, such as subset based DIC, stereo calibration or different kinds of elements.

#### References and Notes

[^A]: Volumetric Digital Image Correlation is not considered here.

[^4]: [ R. Delorme, I. Tabiai, M. Lévesque, The video presents a stable crack growth within a CT specimen laser cut out of a PMMA sheet.](https://figshare.com/articles/CT-PMMA-x5_mp4/7766729)

[^5]: [muDIC documentation, A crash-course in Digital Image Correlation](https://mudic.readthedocs.io/en/latest/intro/dic_intro.html)

[^7]: Tabiai I., Delorme R., Therriault D. et al. [In-situ Full Field Measurements During Inter-Facial Debonding in Single Fiber Composite Under Transverse Load](https://link.springer.com/article/10.1007/s11340-018-0429-9). Exp Mech 58, 1451–1467 (2018)

[^8]: Olufsen, Sindre Nordmark, Arild Holm Clausen, Dag Werner Breiby, and Odd Sture Hopperstad. [X-Ray Computed Tomography Investigation of Dilation of Mineral-Filled PVC under Monotonic Loading](https://doi.org/10.1016/j.mechmat.2019.103296). Mechanics of Materials 142 (March 1, 2020): 103296.
