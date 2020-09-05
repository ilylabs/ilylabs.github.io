---
title: "The behavior of heterogeneous materials"
excerpt: "Characterization tools for a better understanding of the behavior of complex materials"
sidebar:
  - nav: "projects-heteromaterials"
header:
  teaser: /assets/images/projects/ComplexMats/macrocystis.gif
category: [Heterogeneous materials]
tags: [full field measurements, digital image correlation, composite materials, non local modeling]
partners: 
project_date: 2020-2024
contribute: 
learn_more: 
shared_library: 
recruiting: 
last_modified_at: 2020-06-23T12:17:43-04:00
usemathjax: true 
---

## "Complex" (composite) materials

The profitability of the aerospace, automotive, energy and many other industries is strongly linked to a reduction in the energy footprint and mass of the structures involved, while maintaining a similar level of performance and safety. To achieve these objectives, composite materials are increasingly being used. 
However, our ability to predict the behavior of these materials in various situations is still limited, as it has been demonstrated by the ["World Wide Failure Exercise"](https://www.sciencedirect.com/science/article/pii/B9780080444758500020). During the last century, colossal efforts have been put in place to characterize the microstructure of certain metals and understand how they behave. Today, this work allows engineers to predict with robustness and precision the behavior of a metallic part and to take advantage of its microstructure to better perform a specific function. For composite materials, the bulk of this work still remains to be done. 
{: style="text-align: justify;"}

<figure class="half">
    <a href="https://figshare.com/articles/CT-PMMA-x5_mp4/7766729"><img src="/assets/images/projects/ComplexMats/muDIC-CTcrack.gif"></a>
    <a href="https://link.springer.com/article/10.1007/s11340-018-0429-9"><img src="/assets/images/projects/ComplexMats/muDIC-Spec048-eyy.gif"></a>
</figure>
*Left: Stable crack growth within a CT specimen laser cut out of a PMMA sheet, the vertical strain field was measured using Digital Image Correlation, the applied load is also along the vertical direction [^4] . Right: Interfacial debonding of a 1mm in diameter PTFE fiber embedded in Epoxy, tensile load is applied along the vertical direction[^7].*
{: style="text-align: center; font-size:13px; font-style: italic;"}

In order to use composites more efficiently, it is essential to better characterize their behavior at the microstructural level. [Digital Image Correlation (DIC) is a very interesting method that has been recently successfully applied to accomplish just that](/projects/ComplexMats-openDIC), however, **it is not the only one**. DIC provides a way to accurately measure the local behavior of a material, it is thus possible to measure the strain level for any pixel of a picture of the specimen taken during an experiment. The two following animations show some of the results that can be obtained using DIC, the strain levels are shown for the whole specimen and their evolution through time can be seen. Since composite materials have **heteroegenous mechanical properties** because of their **heterogeneous** nature, a characterization method capable of local measurements is required.
{: style="text-align: justify;"}

## Heterogeneous materials

Fiber reinforced composite materials are not the only kind of materials for which characterization that provide accurate local behavior measurements are required. Any material that exhibit a heterogeneous mechanical behavior will require such advanced characterization techniques. I'll be sharing here some of the other fields for which such techniques are quite relevant.
{: style="text-align: justify;"}

### Biological materials

Biological materials are ... weird. They *grow*, and re-grow, they shrink when they aren't happy and are hard to keep happy during an experiment that aims at characterizing behavior. Plus, they're full of water nothing really stick to them most of the time. Because of these reasons (and many others), characterizing these materials using *traditional methods* has always been challenging. DIC however is able to provide strong insights about the behavior of these materials.
{: style="text-align: justify;"}

#### Kelp growth (*Macrocystis*)

This completely unexpected experiment was possible thanks to [Pr. Frédérick Gosselin](http://fgosselin.meca.polymtl.ca) who provided the financial support to connect our DIC capabilities with the intriguing work [Pr. Patrick Martone](http://www3.botany.ubc.ca/martone/) is pursuing around kelp in his lab at the University of British Columbia. The figure below presents one of the experiments we worked on that aimed at measuring the growth of a *macrocystis* over a few hours. These experiments were done in direct collaboration with (soon to be Dr.) [Liam Coleman](https://twitter.com/kelpfiction) and [Joani Viliunas](https://twitter.com/JoaniViliunas).
{: style="text-align: justify;"}

<figure class="half">
    <a href="http://www3.botany.ubc.ca/martone/"><img src="/assets/images/projects/ComplexMats/macrocystis-setup.png"></a>
    <a href="http://www3.botany.ubc.ca/martone/"><img src="/assets/images/projects/ComplexMats/macrocystis.gif"></a>
</figure>
A piece of *Macrocystis* (close to the stipe ("*root*"), as the fastest growth is expected there) is cut and put inside of a plastic contained full of cold sea water[^A]. The specimen is covered with a fine speckle pattern that can stick to kelp (nothing sticks to kelp, they're so slimy) and a microscope is used to take pictures at regular intervals of this area. Lights are necessary to take nice pictures and for kelps to grow. The results are shown in the right image, the principal strain is shown as a contour plot, the lines show the principal direction for each pixel. The scalebar represents a length of 2 mm.
{: style="text-align: center; font-size:13px; font-style: italic;"}

The contour plot animation shown in the right figure showed us *extension* (since we're not 100% certain it's growth - yet) happening all over the stipe along the vertical direction mainly at first. The stipe is expected to grow horizontally while growing vertically at the same time. Later on during the test, significantly stronger extension is registered toward the bottom left corner of the image. It is still unclear if this is because of unequal growth[^B] or because the light was stronger on the bottom side of the image than the top one (more light could stimulate stronger growth). Although the experiments did not provide us with definitive answers, it revealed the strong potential of DIC (and other full field measurement methods) to better understand the behavior of biological materials.
{: style="text-align: justify;"}

#### Pork lungs inflation/deflation

Another interesting experiment I was able to contribute for was the study of pork lung behavior during inflation/deflation. The experiment was led by [Bénédicte Bonnet](https://ca.linkedin.com/in/b%C3%A9n%C3%A9dicte-bonnet-7b14a065/en-us) (now a happy Msc in biomedical engineering). The study was supervised by [Pr. Frédérick Gosselin](http://fgosselin.meca.polymtl.ca), [Pr. Isabelle Villemure](https://www.polymtl.ca/expertises/en/villemure-isabelle) and [Dr. George Rakovich M.D.](https://www.ctsnet.org/home/grakovich). 
There will be a publication for this one, it is currently under review.
{: style="text-align: justify;"}

<figure class="half">
    <a href="">
      <div style="width:100%;height:0px;position:relative;padding-bottom:56.250%;"><iframe src="https://streamable.com/e/4c0a7r?autoplay=1" frameborder="0" width="100%" height="100%" allowfullscreen style="width:100%;height:100%;position:absolute;left:0px;top:0px;overflow:hidden;"></iframe></div>
    </a>
    <a href=""><img src="/assets/images/projects/ComplexMats/lungs.gif"></a>
</figure>
A pair of pork lungs donated to science are connected to a piston that is controlled to inflate and deflate them. A stereoscopic DIC setup (white paint base with a speckle pattern on the lung, two cameras and lights) is used to capture consecutive stereo-images that are used for DIC analysis. The animation on the right presents a contour plot showing the evolution of the strain in the principal direction on the lung's surface.
{: style="text-align: center; font-size:13px; font-style: italic;"}

The animation on the right shows that lungs do not deform homogeneously during inflation, some areas are much more solicited that others, while some areas barely deform. The complete study aimed at studying the impact of lung resection (cutting and removing a section of lung) that is often sealed using a special staple gun. DIC was in this case able to reveal the impact of stapling lung tissue close to the staples and further away from them. 
{: style="text-align: justify;"}

### 3D-printing and films

3D-printed parts and films are also quite hard to characterize, for different reasons. Standard tools cannot be used to measure the deformation of films, while 3D printed parts crack and withstand strong local deformations. Once again, DIC can be used to provide a better understanding of how these materials deform under loading. 3D printed parts (or any kind of **welded** part) will exhibit local behavior along and around the welded area (or the layers for 3D printed parts). 
{: style="text-align: justify;"}

<figure class="half">
    <a href=""><img src="/assets/images/projects/ComplexMats/efep.gif"></a>
    <a href=""><img src="/assets/images/projects/ComplexMats/chitosan.gif"></a>
</figure>
The animation on the left presents the evolution of the deformation along the vertical direction for a 3D printed EFEP (fluoropolymer) dogbone under tensile (vertical) loading. This material exhibits a final strain of more than 300% during a tensile test, and with DIC it is possible to quantify the different behavior of various areas of the specimen during the test. This work was done with [Yahya Abderrafai](https://www.linkedin.com/in/yahya-abderrafai-5207953b/?originalSubdomain=ca) and [Charlotte Weever](https://www.linkedin.com/in/charlotte-weever-1117307b/) for 3D-TRIP. The animation on the right presents a contour plot of the strain along the principal direction of a pre-cracked chitosan *(thick)* film. It is possible to quantify the strain at the tip of the crack although the experiment was quite challenging because the film was transparent (an extremely fine speckle pattern was also necessary). This work was done in collaboration with [Qinghua Wu](https://www.linkedin.com/in/qinghua-wu-90968210b/).
{: style="text-align: center; font-size:13px; font-style: italic;"}

Although DIC is an extremely interesting method for full field characterization of heterogeneous materials, it is still a limited method. Current DIC algorithms are limited around cracks and discontinuities that are often always part of heterogeneous materials.
{: style="text-align: justify;"}

## Other characterization methods

Although DIC is becoming quite popular when it comes to the characterization of composite materials (for other kinds of heterogeneous materials, it often remains anecdotic), it is not the only interesting method that could be adapted to provide novel and relevant insights about the behavior of heterogeneous materials. The two following videos present two examples of methods that could be adapted in laboratories to study and quantify the behavior of various very interesting heterogeneous materials.
{: style="text-align: justify;"}

<figure class="half">
    <a href=""> <iframe width="560" height="315" src="https://www.youtube.com/embed/aDzMB27XVxQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> </a>
    <a href=""> <iframe width="560" height="315" src="https://www.youtube.com/embed/MfHjYKjaLcc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> </a>
</figure>


## Notes and references

[^A]: Kelps need cold sea water to grow and if you're off by a few °C, they won't grow, that's why +2°C in our oceans is so critical. I learned it the hard ways the first days I tried experimenting with kelp (if you're off by +2°C it's not OK).

[^B]: Macrocystis exhibits a wrinkly belt like shape once it grows, it is believed that the wrinkles might appear because of unequal growth between both halves of the macrocystis.

[^4]: [ R. Delorme, I. Tabiai, M. Lévesque, The video presents a stable crack growth within a CT specimen laser cut out of a PMMA sheet.](https://figshare.com/articles/CT-PMMA-x5_mp4/7766729)

[^7]: Tabiai I., Delorme R., Therriault D. et al. [In-situ Full Field Measurements During Inter-Facial Debonding in Single Fiber Composite Under Transverse Load](https://link.springer.com/article/10.1007/s11340-018-0429-9). Exp Mech 58, 1451–1467 (2018)