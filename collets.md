# Collets

There is not much too say about collets, other than "they come in various sizes and quality". Here's a sample set of collets for the Makita router:

![](.gitbook/assets/collets.png)

From left to right:

* 1/4" Makita collet \(comes with the router in the US version\)
* 6mm Makita collet \(comes with the router in the European version\) with a 6mm to 3.175mm adapter inserted
* 1/8" \(3.175mm\) cheap unbranded collet
* 1/4" \(6.35mm\) precision collet from Elaire Corp
* 1/8" \(3.175mm\) precision collet from Elaire Corp

The main constraint with router collets is that the range of available sizes is quite limited, especially toward the smaller diameters. Spindle users have access to "ER" collets that are available in many more sizes, but that is a story for the [Spindle upgrade]() section. 

Using a **collet adapter/reducer** is generally not recommanded as it tends to increase **runout**, but for most jobs it will still work fine.

Below is a short overview of what runout is, but overall this is not something new users have to worry about. 

## Runout / TIR

In theory, the rotation axis of the router shaft \(in black\), the axis of the collet \(in blue\), and the axis of the endmill \(in green\) are aligned: 

![](.gitbook/assets/runout_theory.png)

But in practice, manufacturing tolerances are such that there are small imperfections at all levels:

* the router shaft itself may not rotate perfectly on its axis
* the collet geometry may not be perfect, introducing a misalignment of the axis between the outer surface \(attached to the router shaft\) and the inner surface \(holding the endmill\)
* the endmill itself may not have a perfectly cylindrical shape

The end effect is that the movement of the endmill's tip in the material is the combination of the rotation along its own axis and other unintended deviations. Here's a very \(very\) exagerated view of what happens when cutting a single slot:

![](.gitbook/assets/runout_when_slotting.png)

The expected width of the slot is the endmill diameter, but the actual width of the slot is the sum of the endmill diameter and the amount of deviation \(runout\). This deviation can characterized by the ****maximal displacement measured at a given position on the surface of the endmill as it rotates \(again, extremely exagerated on the view below\): 

![](.gitbook/assets/runout_measurement_sketch.png)

Refer to the section about [Managing runout](measuring-runout.md) for more.







