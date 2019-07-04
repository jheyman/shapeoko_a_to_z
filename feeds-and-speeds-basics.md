# Feeds & speeds

The whole "feeds & speeds" topic is arguably the most daunting part of learning CNC.

"**Feeds**" is feedrate, i.e. the speed of the gantry pushing the endmill into the material.

"**Speeds**" is the rotation speed of the endmill, i.e. RPM.

The reason these words are used in conjunction is that their values are tightly coupled, so what really matters is the _combination_ of feedrate and RPM for a given situation. Actually, they are also somewhat coupled with a number of other parameters \(e.g. depth and width of cut\), so "feeds and speeds" is often short for "_the many cutting parameters that make for a good cut_". 

When first starting CNC, determining adequate cutting parameters feels a little bit like this:

![](.gitbook/assets/feed_speed_magic.png)

Using a proper feeds and speeds and depth/width of cut values is important to :

* get a good quality of the cut \(e.g. surface finish, accuracy\)
* increase tool life \(i.e. keep tool wear to a minimum\), or at least avoid breakage.
* avoid/minimize chatter \(the horrendous sound heard when the endmill/machine vibrate while going through the material\)
* optimize material removal rate \(e.g. how long it takes to cut that 1 inch deep pocket\)

While there is definitely a good amount of experience involved in finding the perfect feeds and speeds for any given situation, there are a few underlying principles that are worth understanding for two reasons:

* to figure out reasonable values to start from, when a new situation shows up for which you cannot find any predefined recommanded values.
* to be in a position to understand how to tune the cutting parameters to achieve the desired result.

{% hint style="info" %}
This section includes a _little_ math \(nothing fancy\), but not to worry: calculators will take care of that for you, and chances are you will ever only have to do simple linear adjustments to existing values manually.
{% endhint %}

Before diving into the relation between feedrate, RPM, and the other parameters, let's check how the tool cuts into the material.

## Conventional milling

In so-called "conventional" milling, the direction of the endmill movement is such that the flutes bite from the inside to the outside of the material. In the sketch below, imagine the blue triangle represents one flute of the endmill. If the blue cross is the position of the center of the endmill when this flute starts biting into the material, and the endmill is moving into the material at feedrate F, then a little bit later the endmill center is at the position of the purple cross, and the cutting tooth has rotated and gone out of the material. The resulting chip of material that was cut during that time is the green part.

It starts out very thin, and gradually increases in thickness. The maximum thickness is when the cutting tooth exits the material, noted "C" below, it is typically called the "feed per tooth" or "chipload per tooth", or usually just "**chipload**".

![](.gitbook/assets/chipcut_conventional.png)

## Climb milling

"Climb" milling is when the direction of the endmill movement is such that the flutes bite from the outside to the inside of the material. In this situation, the flute first bites a large chunk of material \(blue position\), and as the endmill rotates and moves to the left at feedrate F, the cut gets thinner, until the tooth has nothing left to cut \(purple position\). The resulting chip \(in green\) has a similar shape than in conventional milling, and again the max thickness of the chip is the chipload.

![](.gitbook/assets/chipcut_climb.png)

Climb milling is harder on the machine, so it requires a good rigidity \(which is somewhat limited on a stock Shapeoko, and is what it is...\) and strong workholding \(which is the part one can do something about, using a proper clamping system for example\)

## Chipload vs. Feeds and Speeds

So why does chipload matter ? 

A large chipload requires a lot of torque from the router/spindle, and a lot of power/rigidity from the machine \(i.e. there's always a limit to the size of the bite you can take, whether you're a squirrel or a white shark\)

A \(too\) small chipload is not good either, because while cutting through the material the endmill heats up, and this heat needs to go somewhere. The chip carries some of the heat as it flies away, and the amount of heat it removes is linked to its size. So if the chip is too small, heat accumulates on the endmill, which reduces tool life and the quality of the cut. Also, if the chipload gets really small, and since the cutting teeth are not infinitely sharp, instead of slicing into the material the tooth will just rub against it, and then "heat happens".

So again this is a goldilocks situation: the chipload must be high enough to avoid rubbing and overheating of the endmill, and small enough to be compatible with the power/rigidity limits of the machine.

Endmills often have several cutting teeth, and each one contributes in turn to removing material during one revolution of the tool. If we sketched the successive bites that the endmill takes into the material, this would look something like this:

![](.gitbook/assets/chipload_per_revolution800.png)

If the endmill has N flutes, one revolution will cut N chips, i.e. a length of N x  chipload. Since the endmill revolves at RPM turns per minute, in one minute N x chipload x RPM will have been cut. And the distance being cut per minute is exactly the definition of feedrate, therefore Feedrate = N x RPM x Chipload, which also means:

$$
Chipload = \frac{Feedrate}{Nb\_Flutes * RPM}
$$

This relation is quite intuitive:

* for a given endmill and RPM, the faster the feedrate the larger the chipload.
* for a given feedrate, an endmill with more flutes will cut thinner chips 
* for a given feedrate and endmill, the faster the endmill rotates the thinner each chip is 

The important takeway here, is that there are multiple combinations of feedrate, endmill type, and RPM that produce the same chipload. Say you are using a feedrate of 1000mm/min, and a 3-flute endmill at 10.000RPM. Given the formula, you may just as well use a 2-flute endmill at 15.000RPM, or keep the 3-flute endmill but spin it at 20.000RPM while increasing feedrate to 2000mm/min, the chipload thickness will be the same:

$$
\frac{1000}{3 * 10.000} =  \frac{1000}{2 * 15.000} =  \frac{2000}{3 *20.000} =0.033mm
$$

The feedrate/RPM values are consequences of the desired chipload value, so let's first have a look at what a good chipload value is.

## Shapeoko chiploads guideline

Optimal chipload depends on a lot of things, but mostly:

* the \(hardness of the\) **material** being cut
* the **diameter** of the endmill \(smaller teeth need to take smaller bites\)
* the **rigidity** of the machine: it is quite easy to forget that the Shapeoko is a hobbyist-grade CNC and is not as rigid as the professional CNCs, so general recommanded chiploads may not be suitable for the Shapeoko.

This last element is what makes optimal chipload a bit difficult to predict: not only is the Shapeoko different \(less rigid\) than pro CNCs, there can also be differences in rigidity between two Shapeokos depending on how they were assembled/setup, whether they have any mod, etc...

So it is almost impossible to come up with a rule that will provide the _optimal_ chipload for all machine & usecases directly. What is possible though, is to provide a guideline as to the _minimal_ reasonable chipload that can be used as a starting point, and a process to optimize it on a given machine for a given usecase .

You can look for for recommanded chipload values from endmill manufacturers, but they usually assume you are using their endmill on a pro machine, which is MUCH more rigid. For the Shapeoko we need to dial back these chiploads. Unfortunately there is no scientific rule to do this, so let's use a few \(over-\)simplification assumptions:

* chipload scales linearly with endmill diameter
* a commonly seen value for cutting aluminium on a Shapeoko is 0.001" for a 1/4" endmill
* at the other end of the chipload spectrum, the max reachable chipload for a Shapeoko when using a 2-flute 1/4" endmill and lowest RPM \(10.000RPM on the Makita router\), given the Shapeoko's limit to 200 inch per minute feedrate, is 200/\(2x10.000\) = 0.01"
* the ratio of chiploads between hard woods and soft woods should be around the same as the wood hardness ratio \(janka factor\), let's pick an average ratio of about x2 
* Acrylic \(and plastics in general\) require the highest chiploads, because of thermal constraints: plastic can easily melt if heat is not removed quickly enough, and the only way to ensure this is to make large chips \(using low RPM and high feed rate\). Experimental 

If we apply this recipe, we end-up with this guideline for reasonable chiploads on the Shapeoko:

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Soft plastics</th>
      <th style="text-align:left">Hard plastics</th>
      <th style="text-align:left">Soft wood / MDF</th>
      <th style="text-align:left">Hard wood</th>
      <th style="text-align:left">Aluminium</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">Hard plastics + 100%</td>
      <td style="text-align:left">softwood + 20%</td>
      <td style="text-align:left">hardwood + 100%</td>
      <td style="text-align:left">baseline + 100%</td>
      <td style="text-align:left">baseline</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">1/16&quot;</td>
      <td style="text-align:left">
        <p>0.0025&quot;/</p>
        <p>0.06mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0012&quot; /</p>
        <p>0.03mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;/</p>
        <p>0.025mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot;/</p>
        <p>0.0125mm</p>
      </td>
      <td style="text-align:left">
        <p>0.00025&quot;/</p>
        <p>0.0063mm</p>
      </td>
      <td style="text-align:left">half of 1/8&quot; values</td>
    </tr>
    <tr>
      <td style="text-align:left">1/8&quot;</td>
      <td style="text-align:left">
        <p>0.005&quot; /</p>
        <p>0.12mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0024&quot; /</p>
        <p>0.06mm</p>
      </td>
      <td style="text-align:left">
        <p>0.002&quot;/</p>
        <p>0.05mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;/</p>
        <p>0.025mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot;/</p>
        <p>0.0127mm</p>
      </td>
      <td style="text-align:left">half of 1/4&quot; values</td>
    </tr>
    <tr>
      <td style="text-align:left">1/4&quot;</td>
      <td style="text-align:left">
        <p>0.01&quot; /</p>
        <p>0.254mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0048&quot; /</p>
        <p>0.12 mm</p>
      </td>
      <td style="text-align:left">
        <p>0.004&quot;*/</p>
        <p>0.1mm*</p>
      </td>
      <td style="text-align:left">
        <p>0.002&quot;/</p>
        <p>0.05mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;/</p>
        <p>0.0254mm</p>
      </td>
      <td style="text-align:left">baseline</td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
For reference, the absolute maximum chipload that a stock Shapeoko can theoretically achieve when using the fastest feedrate \(200"/min\), the slowest RPM \(10.000 RPM on the Makita router\), and the lowest number of flute \(1\), is  200 / \(1x10000\) = 0.02" = 0.5mm

The highest chiploads in this table may not be reachable using any endmill. For example, the max chipload that can be achieved when using the most common 1/4" endmill \(3-flute 1/4", e.g. Carbide3D's \#201\) given the Shapeoko's limit to 200 inch per minute feedrate and the Makita router min RPM of 10.000 is 200/\(3x10000RPM\) = 0.0066". To get a higher chipload, an endmill with lower flute count, and/or a spindle with lower minimal RPM must be used.
{% endhint %}

## Stepover/WOC/RDOC

"**Stepover**" refers to the offset distance between one cutting pass and the next one, which also translates into how much new material is being removed by the side of the endmill, or how much radial engagement is put on the endmill. It's also called Width of cut \(**WOC**\) or Radial Depth  of Cut \(**RDOC**\)

In the example below, the stepover S if 50% of the endmill diameter:

![](.gitbook/assets/stepover.png)



The wider the stepover, the larger the force on the endmill. Cutting passes with small stepover are better for surface finish quality, while passes with large stepover obviously reduce overall cutting time since less passes are required.

A common rule of thumb is to use:

* **35% to 50%** stepover for roughing using regular toolpath
* **10% to 35%** stepover for roughing using adaptive clearing toolpath 
* **5% to 10%** stepover for finishing

For Ball-end endmills, stepover value influences surface finish quite a lot. Consider the following sketch of a side view showing multiple passes:

![](.gitbook/assets/stepover_ballend_scallop.png)

Scallops of residual material will be left at regular interval on the bottom surface. These will be more or less visible depending on how well the material can hold small details. 

* a 20% to 33% stepover should be small enough for wood
* a 10 to 20% stepover

## Chip thinning

We're almost there, but there is one more thing: the above-mentioned chipload values assume that the cutter circumference is engaged deep enough into the stock, that each tooth gets to cut material from the moment it starts biting \(at the bottom of the blue circle\) to the moment it completes the 90° rotation of its useful activity for that revolution. This happens when the cutter's **radial depth of cut \(stepover\)** is 50% of its diameter:

![](.gitbook/assets/chip_thinning1.png)

Now consider what happens if the radial depth of cut is lower than 50% of the diameter, say 20% only:

![](.gitbook/assets/chip_thinning2.png)

For the same RPM and feedrate, the _actual_ size of the chip that each tooth produces is smaller, because the tooth exits the material early and the rest of its 90° of useful action is not used. The thickness of the chip is lower than expected, which also means that less heat is removed, so this is not optimal.

The solution is to compensate this effect by **increasing the feedrate** \(all other parameters staying the same\), such that the size of the chip is increased to approximately the same amount of material that would have been removed if the cutter was engaged at 50% of its diameter:

![](.gitbook/assets/chip_thinning3.png)

The size of the adjusted target chipload is determined by:

$$
Chipload(adjusted) = \frac{Diameter}{2 * \sqrt{(Diameter * RadialDOC) - RadialDOC^2}}* Chipload
$$

{% hint style="info" %}
For basic toolpaths, the stepover is often in the 40% to 50% range, and it's possible to just ignore chip thinning altogether. Where chip thinning really matters is for adaptive clearing toolpaths, that typically use small stepovers \(more on this in the [Toolpaths](toolpath-basics.md#adaptive-clearing-toolpaths) section\)
{% endhint %}

## Choosing RPM and Feedrate

At this stage, the material is known, the endmill geometry is known, chip thinning is accounted for, which gave us a target minimal chipload. The remaining part is to chose a specific combination of RPM and feedrate values that together will produce this chipload, following the formula described earlier.

Several factors influence which of the equivalent combinations one will choose, but to name a few:

* **RPM limits** of the router: the Makita router will only go as low as 10.000RPM, while the Dewalt can only go down to 12.000RPM. If the recommandation for a given usage is e.g. to use 8.000 RPM with a 3-flute cutter, an alternative will be to use a 2-flute cutter instead while bumping the RPM to 8.000 \* 3 / 2 = 12.000.
* **Cutters geometry**: some usecases call for the use a single-flute endmills, which also means reducing the feerate or increasing the RPM to maintain a proper chipload.
* **Noise**: while using high RPM \(e.g. above 20.000\) has its advantages, on a standard router it is also much noisier, so some may prefer to keep the RPM closer to 10.000, and adjust feedrate accordingly. 

There are basically two approaches:

* **Choosing the desired feedrate** and solving for the associated required RPM value. This is probably not the most likely approach, but may make sense when the intent to maximize the cutting speed.
* **Choosing the desired RPM** and solving for the associated required feedrate value. This is more likely/common because there is usually a greater range of feedrates than the range of RPMs of the router
  * low RPMs are quieter
  * high RPMs induce less cutting forces.

Example:

* Material is hard wood and endmill is 1/4", 3-flute =&gt; look-up in the recommanded chipload table gives us a value of 0.0014"
* Say we use a 25% radial depth of cut / stepover, i.e. 25% of 50% of 1/4" = 0.03125, so adjusted chipload is 

$$
\frac{0.25}{2 * \sqrt{(0.25* 0.03125) - 0.03125^2}}* 0.0014 \approx 1.5*0.0014 = 0.0021
$$

* Let's say I want to use 12.000 RPM, because it is the lowest setting on my router and I prefer to keep the noise down. The required feedrate is:

$$
Feedrate = Chipload*Nb\_Flutes * RPM= 0.0021 * 3 * 12000 = 75  in/min
$$

## Choosing DOC

**Depth of Cut** \(DOC\) also called Depth Per Pass, is how deep into the material the endmill will be while cutting. For a given feedrate and RPM, the deeper it is the larger the forces on the endmill.

Multiple cutting passes at depth of cut d will be required to cut down to a total pocket depth of D:

![](.gitbook/assets/depth_of_cut.png)

DOC is just as important as feeds & speeds to achieve a good cut, yet surprisingly there is much less information about how to determine its value, compared to the abundance of feeds and speeds chart.

The reason is probably that while there are mathematical recipes to choose feedrate and RPM for a given endmill geometry, the achievable DOC is much more tightly linked to the specific machine you are using, and specifically its rigidity and power.

The Shapeoko is not as rigid nor as powerful as pro CNC machines, so DOC recommandations for these machines need to be dialed back, even when using perfect feeds and speeds.

A good rule of thumb is to _start_ with a DOC that is ****around: 

* **5% to 10%** of the diameter of the endmill for metals e.g. **aluminium**
* **10% to 50%** of the diameter of the endmill for **softer materials**

If it cuts fine, then increase DOC gradually to find the limit for your machine/endmill/material.

{% hint style="info" %}
There are two direct implications of having to limit the DOC to relatively small values:

* cutting deep into the material will require many passes, hence of lot of time.
* during each cutting pass, only the tip of the endmill is used, so this part will wear out over time, while the rest of the length of cut is never used.

Check out the **adaptive clearing** strategy in the [Toolpaths](toolpath-basics.md) section, for how to address this.
{% endhint %}

## Slotting

Depending on the stepover, the portion ****of the endmill that will be engaged in the material, a.k.a. the Tool Engagement Angle \(**TEA**\), will be different:

* for a 50% stepover, the TEA will be 90°:

![](.gitbook/assets/tea_90.png)

* for a smaller stepover, say 25%, the TEA will be reduced to 60°:

![](.gitbook/assets/tea_60.png)

**Slotting** is a different story: both sides of the endmill are engaged at all times, so the TEA is 180°:

![](.gitbook/assets/tea_180.png)

The force on the endmill will be twice as much as when cutting at 90° TEA, so the max achievable chipload/DOC combination for a given machine/endmill/material is lower. The recommanded chipload/DOC values above include some margin to take this effect into account to some extent.

The other side effect of slotting is that chip evacuation is not as good: the flutes are in the air only 50% of the time, so the chips that form inside the flutes have less time/opportunities to fly away.  Deep slotting is notorious for causing issues when chips cannot be evacuated quickly enough.

Bottomline: slotting is hard on the machine, so you may have to:

* limit DOC to a smaller value
* limit feedrate \(chipload...\)

{% hint style="info" %}
Or, you can take a different approach and _avoid_ slotting altogether, by using smarter toolpaths. See adaptive clearing and pocketing in the [Toolpaths](toolpath-basics.md#adaptive-clearing-toolpaths) section!
{% endhint %}

Not that you will ever need to use it, but for the math-inclined among you, here's the equation to compute TEA from stepover value:

$$
TEA = \cos ^{ - 1}(1 - \frac{stepover}{0.5*endmill\_diameter})
$$

## Corners

While we are talking about TEA, let's take a look at what happens when cutting a square pocket at 50% tool engagement \(90° TEA\) and reaching a corner:

Just before moving into the corner, the tool engagement angle is 90°:

![](.gitbook/assets/tea_before_corner.png)

But _while_ cutting the corner, the TEA momentarily goes up to 180°:

![](.gitbook/assets/tea_during_corner.png)

before going down to 90° again. So the machine sees a "spike" in the cutting resistance at each corner. Similarly to slotting, the cure is to dial back feedrate and DOC. 

{% hint style="info" %}
But that boils down to optimizing the cut parameters used throughout the job specifically for these very short times when the corners are being cut, which is not very efficient. The alternatives include avoiding straight corners in the design if possible \(e.g. round the corners...\) or use **adaptive clearing toolpath** that will take a lot of very shallow bites at the corners instead of a deep one.
{% endhint %}

## Plunging feeds and speeds

All of the info above only focused on the feeds and speeds for the radial part of the cut, but when the endmill is plunging \(straight down/vertically\), things are quite different:

* \(obviously\) the cutting edges on the circumference of the endmill are not cutting anything anymore, the cutting happens at the tip of the endmill only, like a drillbit.
* But endmills are really not optimized for drilling, so their ability to plunge efficiently through material is quite limited

One could compute specific plunge rate and RPM based on the specific geometry of the tip of the endmill, but in practice it's easier to just:

* use the same RPM as for radial cuts. This is a given when using a router where there is no dynamic control on the RPM anyway, so the same value is used throughout the cut. It would not make much sense either on a spindle to accelerate/decelerate the RPM
* use a plungerate that is experimentally chosen, following the rule of thumb 
  * **25% to 50% of the feedrate for wood and plastics**
  * **10% to 20% of feedrate for metals.**

{% hint style="info" %}
These numbers are for plunging straight down. If the toolpath uses some kind of ramping at an angle into the material, they can be increased quite a bit.
{% endhint %}

## Deflection

The previous paragraphs should have highlighted that MANY factors influence the selection of adequate feeds & speeds & DOC settings. But wait, there are more. For example, **tool deflection** needs to be taken into account. Here is a grossly exagerated sketch of an endmill being subject to the cutting force:

![](.gitbook/assets/tool_deflection.png)

The amount of deflection depends on the endmill material \(carbide is more rigid than HSS\), diameter \(larger is stiffer\), stickout length, and of course the cutting forces that the endmill is subjected to, that depend on the chipload and DOC and materia being cut.

When using conventional milling, the force tends to be parallel to the stock:

![](.gitbook/assets/tool_deflection_conventionalcut.png)

When using climb milling, the force tends to be perpendicular, i.e. push the endmill away from the material:

![](.gitbook/assets/tool_deflection_climbcut.png)

Either way, too much deflection is bad:

* moderate deflection will affect accuracy \(pieces will cut slightly larger or smaller than expected\)
* excessive deflection will cause tool wear or even tool breakage

So this is yet another parameter to take into account in the feeds and speeds and DOC values, especially when using small diameter endmills.

{% hint style="info" %}
Check out the "roughing  and finishing" topic in the[Toolpath basics](toolpath-basics.md) section for ways to mitigate deflection
{% endhint %}

## Wrapping up: suggested process

A proposed workflow to determine a _reasonable starting point_ for feeds and speeds and DOCs on the Shapeoko for a given project that uses a specific **material** and **endmill,** is:

* look-up the **desired minimum chipload** for this material+endmill combination:
  * see guideline table
* choose a **radial depth of cut / stepover:**
  * **35% to 50%** stepover for roughing using a regular toolpath
  * **10% to 35%** stepover for roughing using an adaptive clearing toolpath 
  * **5% to 10%** stepover for a finishing toolpath
* if stepover is less than 50%, **adjust target chipload for chip thinning** 
* select **target RPM** 
  * somewhat arbitrarily within your router RPM range:
    * slower is quieter \(and better for plastics\)
    * higher is better to reduce cutting forces
* determine the specific **required feedrate** for this RPM to achieve the adjusted target chipload
  * Feedrate = adjusted target chipload \* Nb\_Endmill\_Flutes __\*  RPM
  * if computed feedrate exceeds the Shapeoko limit, choose a lower RPM value and recompute feedrate.
* determine **plungerate:**
  * **10 to 50%** of feedrate depending on material hardness
* determine **depth of cut:**
  * **5% to 10%** of the diameter of the endmill for metals e.g. **aluminium**
  * **10% to 50%** of the diameter of the endmill for **softer materials**
* if possible, check **deflection** to make sure there is no risk to break the tool

Then **experiment**: run a cut with those parameters, and incrementally increase chipload \(i.e. feedrate, if keeping RPM constant\) and/or DOC, until chatter or poor finish quality occurs, then dial back feedrate and DOC by 10% and you should be in the sweet spot!

## Telltale signs of wrong F&S

The most common signs of inadequate feeds and speeds are:

* sound, and specifically **chatter**: when feeds and speeds are not right for a given material/endmill/DOC, the tool tends to vibrate, and this vibration can get worse if there is resonance with another source of periodic variation elsewhere in the system \(most often: the router and its RPM\). This results in an ugly sound, a poor finish with marks/dents/ripples on the surface, and a reduced tool life.
* **finish quality**: even without chatter, a poor surface finish can indicate that the final cutting pass was too agressive \(high chipload\)
* **melted material**: especially in plastics and soft metals, if the RPM is too high and/or the feedrate/resuting chipload is too low, the friction will cause the material to melt rather than shear, the tool flutes will start filling with melting material, and this usually ends up with tool breakage.
* **endmill temperature**: the endmill just not be more than slightly warm at the end of a cut: if it got hot to the touch, the feeds and speeds are likely not correct. In extreme cases, the endmill color itself may change to a dark shade
* making **dust**, not chips when cutting wood \(MDF is an exception, but that's not really wood\)

## Productivity-oriented optimization

{% hint style="warning" %}
 If you "just" want a good cut, everything described previously should be enough to determine a good set of cutting parameters, so you can then ignore the following, it's complex enough as it is!
{% endhint %}

If you need to optimize **cutting time** for a given piece,  you will also need to take a look at the **material removal rate** \(MRR\):

$$
MRR = WOC * DOC * Feedrate
$$

This yields a value in cubic inches \(or cubic millimeters\) of material removed by minute, and therefore illustrates how fast you can complete a job. There is always a compromise to be found between going faster but with a lower tool engagement \(low DOC and/or low WOC\), or going slower but with a higher tool engagement \(higher DOC or high WOC\), while staying within the bounds of what the machine can do. The interesting thing about the MRR figure is that it allows to **compare** those different combinations, and figure out which one is the most efficient \(time-wise\).

Now if you want to figure out how close you are to the absolute/physical **limits** of the Shapeoko, \(yet\) another formula comes in the picture, to characterize the required power at the endmill level to achieve this MRR:

$$
HorsePower\_cutter= \frac{MRR}{Material\_K\_factor}
$$

the "K" factor is a constant that depends on the material's hardness, and corresponds to how many cubic inches per minute \(or cubic millimeters per minute\) of material can be removed using 1 horsepower.

For example, 

* 6061 T6 aluminium has a K of 3.34 cubic inches per minute
* it's about 10 in3/min for hard woods ahd hard plastics
* and up to 30 in3/min for soft woods, MDF, ...

Once you get this power value, you can compare it to the Shapeoko's router maximum power, which is rated at a max of 1.25HP \(932Watts\), but is more realistically in the 500W ballpark.

## Calculators

This section should have highlighted that MANY factors influence the selection of adequate feeds & speeds & DOC settings. 

So at some point, it becomes impossible to produce feeds & speeds charts for every possible combination of factors, and also very difficult to compute everything manually. A number of calculators have been implemented to address this, ranging from free excel spreadsheets that basically implement the equations mentioned above, to full-fledged commercial software, the most famous one probably being **G-Wizard.**

Whether or not you _need_ a feeds & speeds calculator is debatable: most people use a limited number of combinations of material/endmill sizes anyway, in which case relying on a few good feeds & speeds & DOC recipes for your machine is enough. The real value of calculators is in **optimizing** the feeds & speeds to achieve a better material removal rate or finish quality \(by keeping deflection under control\) or tool life, etc...

