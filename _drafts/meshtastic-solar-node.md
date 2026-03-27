---
layout: single
title: "Building a solar-powered Meshtastic node that survived winter outdoors"
category: [posts]
tags: meshtastic lora iot maker electronics 3dprinting
date: 2026-03-26 09:00:00
excerpt: "A hands-on build log of a solar-powered outdoor Meshtastic LoRa node using a Seeed XIAO nRF52840 + Wio-SX1262, three 18650 cells, a recycled WiFi camera solar panel, and a 3D-printed hermetic enclosure. It went out in winter. It came back alive."
---

I have been playing with [Meshtastic](https://meshtastic.org/) for a while now, and at some point the obvious question came up: can I put a node *outside*, *permanently*, running on solar, with no maintenance? Turns out the answer is yes — but it took a few iterations to get there.
{: style="text-align: justify;"}

This post is the build log of a simple, low-cost outdoor solar node that I assembled, deployed in winter, and am happy to report is still running. If you want to skip the theory and jump straight to the build, scroll to the hardware section.
{: style="text-align: justify;"}

---

## What is Meshtastic?

Meshtastic is an open-source project that turns cheap LoRa radio modules into a text-messaging mesh network. No cell towers, no subscription, no internet required — just direct radio. The mesh algorithm floods the first packet to discover a route, then remembers which relay delivered the acknowledgment and prefers it for future packets (what the project calls *Managed Flooding with Next-Hop routing*, introduced in v2.6).
{: style="text-align: justify;"}

The radios operate in ISM licence-free bands (868 MHz in Europe, 915 MHz in North America), use AES-256 encryption by default, and the default `LongFast` preset (SF11, 250 kHz bandwidth) easily covers several kilometres of open terrain. The world record ground-to-ground contact is 331 km — mountains and good antennas help — but realistically, a well-placed node in your neighbourhood adds 5–15 km of reliable relay range to the local mesh.
{: style="text-align: justify;"}

The project has around 40,000 active nodes worldwide and is growing fast in maker communities. It is genuinely useful for hiking, rural areas, events, and anywhere you want off-grid communication that is not dependent on any third-party infrastructure.
{: style="text-align: justify;"}

---

## Hardware choices

<!-- PHOTO: component flatlay — all parts before assembly -->

### The radio: XIAO nRF52840 + Wio-SX1262

Seeed Studio sells a kit pairing their **XIAO nRF52840** board with a **Wio-SX1262** LoRa module, pre-flashed with Meshtastic firmware. This combination is hard to beat for an always-on solar node.
{: style="text-align: justify;"}

The nRF52840 is an ARM Cortex-M4F running at 64 MHz with 256 KB of RAM and Bluetooth 5.0 built in. Its key advantage over the ubiquitous ESP32-based alternatives is power consumption: the nRF52840 sleeps at around 10–16 µA, while ESP32 sleep currents tend to be 10–100× higher. For a solar node that may see limited sun in winter, that gap matters.
{: style="text-align: justify;"}

The SX1262 LoRa chip adds a TCXO — a temperature-compensated crystal oscillator — which keeps the radio frequency stable as the enclosure heats up during the day and cools down at night. Without frequency stability, LoRa links degrade in temperature extremes. This is a quiet but important feature for any outdoor deployment.
{: style="text-align: justify;"}

### Power: 3× 18650 in parallel + WiFi camera solar panel

Three 18650 cells wired in parallel give roughly 9,000–10,500 mAh at 3.7 V. At typical outdoor node activity levels (mostly sleeping, waking to relay packets or beacon every few minutes), the average current draw is in the 1–5 mA range. That translates to weeks of runtime without any solar input.
{: style="text-align: justify;"}

The solar panel is a recycled WiFi camera panel — the kind that ships with cheap outdoor security cameras. These are typically rated around 2–5 W and are already weatherproofed. Repurposing one saves cost and it is sized well for this application. Even on overcast winter days a small panel trickle-charges the pack enough to keep the node running indefinitely.
{: style="text-align: justify;"}

### Antenna: 20 cm wire monopole

At 868 MHz, 20 cm is approximately a 5/8-wavelength monopole. That geometry slightly compresses the radiation pattern compared to a quarter-wave, directing more energy toward the horizon rather than skyward — exactly what you want when your counterpart nodes are at ground level. The gain is modest (around 3–4 dBi), but the choice is principled and practical.
{: style="text-align: justify;"}

### Enclosure: 3D printed + silicone

<!-- PHOTO: the finished enclosure before deployment -->

The enclosure is 3D printed in ASA, which is the community-recommended filament for outdoor nodes. ASA is UV-stable and rated down to −40°C, unlike PETG or PLA which can become brittle or deform in sun and cold respectively. The lid joint is sealed with a bead of silicone, and cable entries use silicone-potted glands. Pressure cycling from daily temperature swings can pump moisture through a marginal seal over months — the silicone does the work here.
{: style="text-align: justify;"}

---

## The build

<!-- PHOTO: assembly in progress — PCB, batteries, and wiring inside the case -->

Assembly is straightforward once you have the parts. The nRF52840 + SX1262 kit is compact enough to fit comfortably alongside the batteries in a medium-size printed enclosure. I ran the solar panel wires through a gland at the back and tied the antenna lead to a short pigtail exiting through a second gland at the top.
{: style="text-align: justify;"}

The 3D-printed case was designed in two halves. The bottom half holds the battery holder and PCB on standoffs; the top half is the lid. A continuous silicone bead on the mating flange provides the seal. Four M3 screws clamp the assembly together. The whole build took an afternoon.
{: style="text-align: justify;"}

---

## Winter deployment

<!-- PHOTO: the node mounted outdoors, winter context visible -->

I put the node outside in winter, mounted at height. Here is what I observed:

**Solar and battery performance:** The battery held well throughout the season. The solar panel recharges the pack efficiently, even on partly cloudy days. Interestingly, range seemed slightly better on snowy, overcast days — likely because the cold, dense air and reflective snow improved propagation conditions.

**Range and mesh:** The node joined our local Meshtastic mesh and has been relaying traffic reliably. Height is the single biggest factor for range — even a modest elevation above rooftop level adds kilometres of reliable coverage. That said, we need more nodes in the area to fill coverage gaps.

**Reliability:** Meshtastic itself is still maturing. The mesh is not always reliable for guaranteed delivery — packets drop, routes occasionally fail to establish, and the protocol is not designed for critical communications. For casual off-grid messaging and mesh experimentation it is excellent; for life-safety applications it is not there yet.
{: style="text-align: justify;"}

**Maintenance:** None so far. The silicone-sealed ASA enclosure handled repeated freeze-thaw cycles without issues.
{: style="text-align: justify;"}

---

## What I would do differently

- **Add GPS.** A GPS module (like the L76K) makes the node show up on the mesh map and enables position sharing. It adds ~20–30 mA during fix acquisition, but you can duty-cycle it.
- **Upsize the solar panel slightly.** The camera panel works, but a proper 5 W panel would give more headroom for short winter days.
- **Label the enclosure with a contact address.** If the node ever needs servicing or moves, it is useful to have an identifier.

---

## Join the local mesh

If you are in the area and running Meshtastic, your node will likely pick mine up. The more nodes, the better the coverage — if you are thinking about building one, this is a low-cost, low-effort entry point. Happy to answer questions in the comments.
{: style="text-align: justify;"}

Parts used:
- [Seeed XIAO nRF52840 + Wio-SX1262 kit](https://wiki.seeedstudio.com/XIAO_nRF52840_with_Meshtastic/)
- 3× 18650 Li-Ion cells
- Solar panel (repurposed WiFi camera panel, ~3–5 W)
- 20 cm wire antenna
- 3D-printed ASA enclosure
- Silicone sealant
