# Cutters

It's easy to be overwhelmed at first by the variety of tools and their characteristics. Roughly, an endmill is caracterized by:

* its **type** \(see below\)
* the **diameter of its shank** \(the part that goes in the collet/tool holder\)
* the **diameter of its cutting part**
* the **length of its cutting part \(**Length of cut / LOC**\)**, and its overall length \(OAL\).
* the **number of flutes** = number of cutting teeth
* the **material it is made of**, and its **coating** if any
* whether it is **center cutting** or not. Most are, it means it has the ability to plunge into the material \(vertically\), like a drill bit does.
* the **helix angle**, if it's a spiral endmill.

![](../.gitbook/assets/endmill_anatomy.png)

The most common endmills for the Shapeoko have a diameter of 1/4” \(6.35mm\), 1/8” \(3.175mm\), 1/16" \(1.5875mm\), and 1/32" \(~0.8mm\), with some additional metric sizes \(1,2,4, 6 and 8mm\)

**Square** endmill is ****the most common type

* **upcut**: the direction of the flute is such that it pulls chips away from the cutting surface, thus is quite efficient at removing chips. I will produce a nice finish at the bottom of pockets, but can produce tear out on the surface for some materials.
* **downcut**: pushes chips downward when cutting, so not efficient to evacuate them from the cutting area. It tends to do the opposite as an upcut, i.e. leaving produce a clean cut at the surface of the material, but potential tear out at the bottom of pockets.
* **compression**: the flute geometry is such that it combines an upcut section at the bottom of the tool, and a downcut section at the other end of the cutting part, so for certain pocket depths it can give a nice finish both at the bottom and top of pockets.

Here's an example for a 3-flute, 1/4", upcut square endmill \(codename "\#201" on Carbide3D store\)

![](../.gitbook/assets/tools_3flute_025inch.png)

It also comes in a version coated with zirconium nitride \(ZrN\) which is better to cut metals:

![](../.gitbook/assets/tools_3flute_025inch_zrn_coated.png)

Here's an example of a 1/4" endmill with only 2 flutes:

![](../.gitbook/assets/tools_2flute_square_025inch.png)

and here's a 1/4" one with a single flute:

![](../.gitbook/assets/tools_1flute_025inch.png)

**Ballnose** endmills are better suited for machining smooth 3D curves \(but are quite inefficient for machining flat pockets\), here's a 2-flute 1/4" ballnose:

![](../.gitbook/assets/tools_2flute_ballnose_025inch.png)

On the smallest diameter endmills, the cutting length is really short \(otherwise the tool would be extremely fragile\), here's a 0.032"ballnose:

![](../.gitbook/assets/tools_2flute_0032inch.png)

**V-bits** come with various angles, they are typically used to carve text or tiny groove \(more on this in the [Toolpath basics](toolpath-basics.md) section\):

![](../.gitbook/assets/tools_vbits.png)

Then there are even more specialized bits like **surfacing** bits that are aimed at...surfacing material. They usually have a very large diameter, to be able to cover a wide area in a few \(very shallow\) passes:

![](../.gitbook/assets/tools_surfacing_bit.png)

Another specialized bit is the **PCB engraving** one, it's basically a very \(very\) pointy one that can be used to make very thin traces on the top of a copper-clad board.

![](../.gitbook/assets/tools_pcb_engraving_bit.png)

The **diamong drag** bit has a tiny diamond on its tip, and it intended to be dragged across the surface of the material \(with the router TURNED OFF!\) to engrave a 2D pattern. The tip is usually spring-loaded, so that the tip is always in contact with the surface with a controlled pressure, while the bit moves around:

![](../.gitbook/assets/tools_diamond_drag_bit.png)

Even more specialized: **thread milling** bits are used in conjunction with very specific spiral toolpaths, to thread the inside of a hole:

![](../.gitbook/assets/tools_thread_milling.png)

And there are many more, but these should cover a large part of the usecases/projects.





