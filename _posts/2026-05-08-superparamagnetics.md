---
layout: post
title:  "Noodling on Super Magnetic Materials"
unlisted: true
---

### Table stakes:

Power electronics needs magnetics (inductors and transformers) for energy storage, isolation, and voltage conversion. Magnetics store energy in magnetic fields (duh), whereas their brethren in energy storage, capacitors, store energy in electric fields. Capacitors are really good at being energy dense and low loss, but you can't get away with _just_ using capacitors. You need both. That effectively means that a substantial portion of the volume and losses (often the plurality, sometimes the majority) of any power converter is dominated by magnetics. 

<p style="text-align: center;">
<img src="/assets/images/LCgraph.png" width=300>
<br>
<i>Chart comparing energy density of inductors and capacitors. Inductors are worse, but we need them nonetheless.</i>
</p>

So optimizing our magnetics ends up having an outsized impact on total converter performance. One area this particularly matters is in high frequency power converters. In certain applications, we really care about making converters as dense as possible; the prototypical example of this is a server power supply, where engineers are really space constrained. In such applications, we achieve density by switching our converters at high frequencies; this reduces the energy storage needed. Ideally, we'd love to be able to switch at single digit to tens of MHz. The semiconductors can handle it; the capacitors can handle it; but the magnetics can't. The materials aren't good enough! They're too lossy.

### What do we need from our materials?

There's an endless list of metrics that people use to characterize magnetic materials, but I think three really matter: Permeability (which determines inductance); saturation (which determines how much magnetic field you can put in a material before it begins failing); core loss (self-explanatory, losses!). The thing I think the industry has not really internalized is: For really high frequency operations, you can punt on permeabiilty (b/c you need very low inductance) and on saturation (b/c at high frequencies the point is you're trying to store very little energy), and just optimize *core loss*. 

One other thing that matters: manufacturability. The dominant material for these HF applications right now is ferrite. Ferrite has lots of good properties, but is extremely brittle. You have to press into molds, sinter at high temps, then grind things to precision (often cracking in the process, I know this from hard-earned experience). The below image is of a high performance core NVIDIA Research is working on. It would so incredibly hard to make, especially at scale.

<p style="text-align: center;">
<img src="/assets/images/tinyCores.png" width=300>
<br>
<i>Tiny high performance power inductors, from a recent NVIDIA paper.</i>
</p>

### Enter superparamagnetism:

I stumbled into this presentation on superparamagnetic materials at a conference a few months ago. The authors were materials people, not power electronics people. The idea is that if you get tiny nanoparticles of iron oxide to form in a matrix of non-conductive material, you get this fancy property called superparamagnetism: It has *extremely* low losses, but poor (but good enough!) permeability and saturation. And you can make it via casting and additive manufacturing! That's my dream material! These guys figured out how to make it quite consistently in a 2018 paper; but never did proper power electronics characterization for it. They've been looking at using it for biomedical imaging. I want to use it to make the best, most high-performance power inductors in the world.

### General thoughts:

My first startup, I screwed up. I had the research from grad school, and I tried to conjure up market demand for it. Everyone tells you that's the wrong thing to do, and I heard their advice and didn't take it. My bad. As Feynman said, "The first principle is that you must not fool yourself, and you are the easiest person to fool." I think this thing has legs because I'm the guy who wants to use it. I'm starting the other way around, and seeing what the market needs and working backward.
