---
layout: post
title:  "What does my startup, Palanquin Power, do?"
---

This is my description of what my startup, Palanquin Power, is working on. More or less, it's what I tell people if given 15 minutes. I aim for this to be a reference to point interested parties to. 

Before we talk about the details, let's establish some basics. We work on **power electronics**. Imagine your laptop charger. If you were to somehow directly plug your laptop into the outlet with two wires, you'd immediately destroy your computer - the internal electronics aren't designed to interface with the 120V AC that comes from the plug. So we need something (a converter) to *convert* from 120V AC to ~20V DC. That's power electronics! This stuff comes in big packages, like grid tied inverters for utility scale solar, and tiny ones that are on the integrated circuits in your phone. 

OK, so power electronics exist. What about their performance? The headline metrics here are **efficiency** and **density**. Remember your laptop charger is warm to the touch - that's loss! You as a consumer don't care whether that brick is 85% or 95% efficient, but some customers (spoiler - data centers) do. Then there's also how much power it can push through - my Macbook charger can handle 40W in its handheld form factor, but you could imagine a higher performance model that could charge my battery faster in the same volume. The metric here is typically W/cm^3. Generally, applications also care about **cost**, and some niche use cases like aerospace are very **mass** sensitive. These metrics are typically traded off against eachother on some sort of multi-dimensional Pareto front. I can make a 99.9% efficient converter given a very large volume, or conversely, a very dense but very lossy converter.

<p style="text-align: center;">
<img src="/assets/images/paretowhite.svg" width=300>
<br>
<i>3 dimensional Pareto front for power converter design</i>
</p>

Now we're ready to talk data centers. These facilities have roughly three stages of power conversions (note that these details are really only true for *hyperscale* centers):
- There's a grid connection supplying ~tens of thousands of volts AC. This goes into medium voltage tranformers, and spits out lower voltage (hundreds of volts) AC. This is where lots of 'solid state transformer' startups are playing, with ideas on converting directly to +/-400V or 800V DC. 
- 3-phase LV AC (or soon, 800V DC) is distributed to the racks, and fed into a power shelf - a rack mounted unit with several converters inside. This shelf converts from its input to 50V DC output. There can be lots of power shelves with outputs connected in parallel to a big fat bus-bar (a thick piece of copper) that runs along the back of the rack.
- 50V DC goes into the server, but the CPUs and GPUs want less than 1V! So we have power converters - called point of load converters, that sit right next to the chips. This is an area of immense technological interest, as designing high performance converters here is really tricky, given extremely high output currents (2kW / 0.7V = ~3kA!) and space constraints. Again, several startups are working in this space.

<p style="text-align: center;">
<img src="/assets/images/DCDiagram.svg" width=600>
<br>
<i>Rough doodle of data center power architecture</i>
</p>

Palanquin aims to improve the performance of that middle, rack-level stage. Currently available AC power shelves have efficiencies of about 96.5-97.5%, and can process about 72kW per unit. That's really good! Let's understand why a data center would care about improving these metrics. 
- Every data center today and in the future is power limited (that's Jensen Huang's line, not mine). Saving percentage points of power translates directly into deploying more compute, which drives revenue and results in very fast payback time. 
- Power conversion is fighting for space inside the rack with compute infrastructure. With the movement towards deploying 600+kW in a single rack, it's becoming a real need to make converters as dense as possible. This is why we see hyperscalers working toward deploying power conversion in a sidecar rack. 

Of course, winning on both efficiency and density is really hard! But Palanquin's approach uniquely can. 

I'll make an analogy to a bunch of christmas lights, composed of LEDs (note that I'm being a bit loosey goosey with some of the details here, bear with me if you're a electrical engineer). Each LED 'wants' 1 volt accross it, and let's say we have a 5V supply. One thing we could do is build a 5V -> 1V DC to DC converter, and put the supply on one side, and our LEDs on the other. This is a great solution, and roughly analogous to the data center status quo. However, it means that we've introduced a new source of losses & volume into our system. 

An alternative approach would be to take 5 LEDs (or groups of 5 LEDs), and connect them in a string. This way, the voltage drops sum and the power supply 'sees' 5V, since 5 x 1V = 5V. Life is grand! We've gotten rid of that pesky power converter, and eliminated its losses and volume!

<p style="text-align: center;">
<img src="/assets/images/LED1.svg">
</p>

This seems all too cute, and it is. This scheme, implemented naiively as above, would not work. Real world loads, such as servers, do not magically draw a fixed voltage. If you stacked up 8 servers with 50V nominal inputs and connected them to a 400V bus, you'd quickly find that voltage would not evenly distribute across each load, causing catastrophic failures. To solve this, we need to introduce converters back into the system. However, the role of the converter changes. Instead of processing  the total server power (as done in the other case), these converters only process *the difference in power between the loads*. In a situation where your servers are all running at the same power, the situation looks a lot like our Christmas lights again, where the converters almost don't exist (since they process very little power).

<p style="text-align: center;">
<img src="/assets/images/GenericDPPWhite.svg" width=400>
<br>
<i>Differential power processing schematic</i>
</p>

Why is this advantageous? Well, to first order - if your converters process less power, they create less losses! This approach has been experimentally validated to have 99.9% efficiency in small scale tests. Second, if you are are relatively insensitive to the the efficiency of your converters (because they are processing less power), you can pick a different point on the Pareto front to optimize around! So this system can thus win on efficiency **and** density! This approach works the best when all loads are evenly-matched. That is routinely true for AI training - the biggest, nastiest data center load possible. Other approaches' worst nightmare is our ideal operating condition.

A question I normally get is - well, why is this hard then? I've accidentally made it seem too simple! It turns out that implementing this scheme is really quite tricky. We have a lot of power converters that have to accomplish local control objectives very quickly (because servers have nasty, peaky power consumption), as well as preserve global stability! Accomplishing both is a difficult controls problem, and our core IP makes it so that we can. 

So our activities are building unique power converters with novel controls and architecture, for this specific application. Right now we are scaling up our approach, getting data as we test at higher power.

I'll leave it here for now. As you might expect, I'm extremely happy to discuss Palanquin with anyone who's interested (especially if you have money or problems we can solve), so feel free to reach out.


<p style="text-align: center;">
<img src="/assets/images/4Module.png" width=400>
<br>
<i>Current experimental test setup</i>
</p>