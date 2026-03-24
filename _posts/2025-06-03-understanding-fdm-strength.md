---
layout: single
title: "Understanding 3D Print Strength: Notches vs. Bonding"
date: 2025-06-03
author: "Maryam Shokrollahi, Adam W. Smith, Arthur Levy, Martine Dubé, Ilyass Tabiai"
categories: [3D Printing, research]
tags: [FFF, FGF, 3D Printing, PETG, Mechanical Properties, Anisotropy, Interface Bonding, Notches]
---

{: .notice--info}
**Publication associated with this work**: Maryam Shokrollahi, Adam W. Smith, Arthur Levy, Martine Dubé, Ilyass Tabiai, "Reassessing anisotropy in 3D printed structures: The role of extrudate geometry vs interface bonding", Journal of Manufacturing Processes, Volume 149, 2025, Pages 456-472, ISSN 1526-6125, [https://doi.org/10.1016/j.jmapro.2025.05.070](https://www.sciencedirect.com/science/article/pii/S1526612525006322)

<audio controls>
  <source src="{{ site.baseurl }}/assets/audio/Notch_Effects_in_3D_Printed_Structures.mp3" type="audio/mpeg">
  Audio summary courtesy of NotebookLM.
</audio>

## Unlocking the Potential of 3D Printing: New Insights into Mechanical Performance 

3D printing, also known as Additive Manufacturing (AM), is revolutionizing how we create objects.  From prototyping to producing consumer goods, its applications are expanding rapidly.  Two common polymer extrusion-based methods are Fused Filament Fabrication (FFF) and Fused Granular Fabrication (FGF). FFF uses pre-made filaments to create small-to-medium sized objects, while FGF uses polymer pellets to produce larger structures.
{: style="text-align: justify;"}

However, a major roadblock to wider adoption, especially for high-performance applications, is **the limited mechanical strength of the printed parts**.  Our recent research offers novel proofs to determine why these parts aren't as strong as we'd like them to be. 

### The Culprits: Incomplete Bonding and Extrudate Geometry 

<figure style="text-align: center;">
  <iframe src="https://widgets.figshare.com/articles/26980852/embed?show_title=1" width="738" height="456" allowfullscreen frameborder="0"></iframe>
  <figcaption style="text-align: center;"><strong>Figure 1:</strong> Schematic of the main strength reduction mechanisms in FFF/FGF printed parts: a. Fast cooling of the extrudates prevents the complete interdiffusion of the polymer chains at the interface. b. Extrudate geometric features act as stress concentration areas. c. Porous mesostructure is also a result of the oval shape geometry of the extrudate.</figcaption>
</figure>

We've identified two main factors that contribute to this weakness: 

1.  **Incomplete Bonding:** Think of it like trying to glue two pieces of plastic together. If the glue doesn't spread well or dries too quickly, the bond won't be very strong.  Similarly, in 3D printing, the polymer chains in each layer need to properly meld with the layer below.  If they don't, the connection is weak.   

2.  **Extrudate Geometry:** The material is laid down in a rounded rectangular shape. This creates ridges on the surface and voids inside the structure (think of these voids as internal cracks), as shown in Figures 1b and 1c.  These ridges, which we call "interface notches," are like stress concentrators.  When the material is pulled, the stress isn't evenly distributed; it's concentrated at these notches, leading to earlier failure. 

### Separating the Effects 

To truly understand the impact of these two factors, we needed a way to study them independently.  We first eliminated inter-filament porosities by printing single walls.  Then, we used a machining method to carefully remove the interface notches from our 3D-printed samples.  This allowed us to test the strength of the material with and without the notches, effectively isolating the effect of the bonding and extrudate geometry.
{: style="text-align: justify;"}

### Key Findings 

Our experiments, using Polyethylene Terephthalate Glycol (PETG), revealed some critical insights: 

* **Notches are the Main Problem:** The interface notches are the primary cause of weakness.  Removing them significantly increased the strength of the printed parts.  In fact, the ultimate tensile strength increased by 28% in small-scale samples and a whopping 70% in large-scale samples! 
* **Digital Image Correlation (DIC) Shows How Notches Affect Strain:** We used a technique called Digital Image Correlation (DIC) to see how the material deforms under stress.  This showed us that the notches indeed concentrate strain, leading to earlier failure. 
* **Bigger Notches, Bigger Problem**: We also found that the severity of the notches (how deep and sharp they are) is critical.  Larger, sharper notches lead to greater stress concentrations and weaker parts. 
* **Good Bonding is Possible:** When we removed the notches, the material exhibited isotropic properties, meaning it has the same strength in all directions.  This tells us that, under the right conditions, we can achieve full bond strength in 3D-printed parts.
{: style="text-align: justify;"}

### What Does This Mean for the Future of 3D Printing? 

This research provides valuable guidance for improving 3D printing: 

* **Optimizing Print Parameters:** We can use this knowledge to fine-tune printing parameters (like temperature and speed) to minimize notch formation and improve bonding. 
* **Designing Better Machines:** This work can inform the design of next-generation 3D printers that are better at controlling the shape of the extrudate and promoting good bonding between layers. 

By addressing these issues, we can unlock the full potential of 3D printing, enabling the production of stronger, more reliable parts for a wide range of applications.
