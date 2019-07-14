# Feeds & speeds

The whole "feeds & speeds" topic is arguably the most daunting part of learning CNC.

"**Feeds**" is feedrate, on some CNCs with a fixed tool and moving plate this is the speed at which the material is fed into the endmill, on a Shapeoko this is the speed of the gantry pushing the endmill into the material.

"**Speeds**" is the rotation speed of the endmill, i.e. RPM.

"Feeds" and "Speeds" go hand in hand, what really matters is the _combination_ of feedrate and RPM values for a given situation. Actually, they are also somewhat coupled with a number of other parameters \(e.g. depth and width of cut\), so "feeds and speeds" is often short for "all the cutting parameters". 

When first starting CNC, selecting adequate cutting parameters feels a little bit like this:

![](.gitbook/assets/feed_speed_magic.png)

Using a proper feeds and speeds and depth/width of cut values is important to :

* get a good quality of the cut \(e.g. surface finish, dimensional accuracy\)
* increase tool life \(i.e. keep tool wear to a minimum\), or at least avoid breakage.
* avoid/minimize chatter \(the horrendous sound heard when the endmill/machine vibrates while going cutting through the material\)
* optimize material removal rate \(e.g. how long it takes to clear that 1 inch deep pocket\)

While there is definitely a good amount of experience \(and experimentation\) involved in finding the perfect feeds and speeds for any given situation, there are a few underlying principles that are worth understanding for two reasons:

* to figure out reasonable values to **start** from, when a new situation shows up for which you cannot find any predefined recommanded values.
* to be in a position to understand how to **tune** the cutting parameters to achieve the desired result.

{% hint style="info" %}
This section includes a little math \(nothing too fancy\), but not to worry: while it is important to understand the _dependencies_ between the cutting parameters, calculators will take care of computations for you.
{% endhint %}

Before diving into the relation between feedrate, RPM, and the other parameters, let's check how the tool cuts into the material.

## Conventional milling

In so-called "conventional" milling, the direction of the endmill movement is such that the cutting edges bite from the inside to the outside of the material. In the sketch below, imagine the blue triangle represents one cutting edge of the endmill. If the blue cross is the position of the center of the endmill when this cutting edge starts biting into the material, and if the endmill is moving into the material at feedrate F, then a little bit later the endmill center is at the position of the purple cross, and the cutting tooth has rotated and gone out of the material. The resulting chip of material that was cut during that time is the green part.

It starts out very thin, and gradually increases in thickness. The maximum thickness \(noted "C" below\) is when the cutting edge exits the material. It is typically called the "feed per tooth" or "chipload per tooth", or usually just "**chipload**", and this is the cornerstone of feeds and speeds.

![](.gitbook/assets/chipcut_conventional.png)

## Climb milling

"Climb" milling is when the direction of the endmill movement is such that the cutting edges bite from the outside to the inside of the material. In that situation, the cutting edge first bites a large chunk of material \(blue position\), and as the endmill rotates and moves to the right at feedrate F, the cut gets thinner, until the tooth has nothing left to cut \(purple position\). The resulting chip \(in green\) has a similar shape than in conventional milling, and again the max thickness of the chip is the chipload.

![](.gitbook/assets/chipcut_climb.png)

## Chipload vs. Feeds and Speeds

So why does chipload matter so much ? 

A large chipload requires a lot of torque from the router/spindle, machine rigidity, and each endmill has recommanded chipload limits anyway \(i.e. there's always a limit to the size of the bite you can take, whether you're a squirrel or a white shark\)

A too small chipload is worse: since the cutting edges are not infinitely sharp, at some point instead of slicing into the material, the cutting edges will just rub against the surface, and then "heat happens", which is very bad for the quality of the cut and for tool life. In CNC, rubbing is the enemy.

So this is a goldilocks situation: the chipload must be high enough to avoid rubbing and overheating of the endmill, and small enough to be within with the torque/rigidity limits of the machine and rated endmill maximums.

Each flute contributes in turn to removing material during one revolution of an endmill. If we sketched N successive bites that the endmill takes into the material, it would look something like this:

![](.gitbook/assets/chipload_per_revolution800.png)

If the endmill has N flutes, one revolution will cut N chips, i.e. a length of N x chipload. Since the endmill revolves at RPM turns per minute, in one minute a length of N x chipload x RPM will have been cut. And the distance being cut per minute is exactly the definition of feedrate, therefore Feedrate = N x RPM x Chipload, which also means:

$$
Chipload = \frac{Feedrate}{Nb\_Flutes * RPM}
$$

This relation is quite intuitive:

* for a given endmill and RPM, the faster the feedrate the larger the chipload.
* for a given feedrate and RPM, an endmill with more flutes will cut thinner chips.
* for a given feedrate and endmill, the faster the endmill rotates the thinner each chip will be. 

The important takeway here, is that there are an infinite number of possible combinations of feedrate, endmill type, and RPM to reach a given chipload. Say you are using a feedrate of 1000mm/min \(39ipm\), and a 3-flute endmill at 10.000RPM. Given the formula, theoretically you may just as well use a 2-flute endmill at 15.000RPM, or keep the 3-flute endmill but spin it at 20.000RPM while increasing feedrate to 2000mm/min \(79ipm\), the chipload thickness would be the same:

$$
\frac{1000}{3 * 10.000} =  \frac{1000}{2 * 15.000} =  \frac{2000}{3 *20.000} =0.033mm
$$

For a given chipload, some combinations are still better than other mathematically-equivalent ones though \(more on this below\). Since the feedrate/RPM combination is derived from the desired chipload value, let's first have a look at what the range of acceptable chipload values is for the Shapeoko.

## Shapeoko chiploads guideline

For the **minimum** chipload value to avoid rubbing, there is a large consensus in the CNC community that a value of **0.001"** \(0.0254mm\) is a good absolute lower limit guideline, at least for 1/4" endmills and larger. It may need to be lowered to 0.0005" for 1/8" and smaller endmills.

{% hint style="info" %}
This is assuming you are using a **sharp** cutter. You should never use a dull cutter anyway, if you do you may end up rubbing even at this 0.001" chipload.
{% endhint %}

The **maximum** reachable chipload depends on a lot of things, but mostly:

* the **hardness of the material** being cut
* the **type and diameter of the endmill** \(smaller teeth need to take smaller bites: the maximum chipload for an endmill scales linearly with its diameter\)
* the **toolpath** used \(how wide/deep the cutter is engaged\) and the **rigidity of the machine**: it is quite easy to forget that the Shapeoko is a hobbyist-grade CNC and is not as rigid as the professional CNCs, so endmill manufacturers recommandations may not be directly suitable for the Shapeoko. Any mechanical mod of the machine also impacts the max chipload capability.

The Shapeoko's limits must also be accounted for: the absolute maximum theoretical chipload on a stock Shapeoko would be reached when using a single-flute endmill at the lowest RPM \(10.000RPM on the Makita router\) and at the fastest feedrate of 200 inch per minute, and that would be 200/\(1x10.000\) = 0.02" = 0.5mm

So all chiploads should be somewhere between 0.001" and 0.02".

The following is an \(arguable\) table I am using as a personal reference, which I derived from analysis of a large number of feeds and speeds settings shared in the Shapeoko community, as well as my own experimentations. Do not take it for granted, start above 0.001" and increase it incrementally \(by keeping RPM constant and increasing feedrate\) to find the limits for your machine and for a given material.

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Soft plastics</th>
      <th style="text-align:left">Soft wood &amp; hard plastics</th>
      <th style="text-align:left">Hard wood &amp; aluminium</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1/16&quot;</td>
      <td style="text-align:left">
        <p>0.002&quot;-0.003&quot;/</p>
        <p>0.05mm-0.075mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;-0.0015&quot; /</p>
        <p>0.025mm-0.04mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot;/</p>
        <p>0.013mm</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">1/8&quot;</td>
      <td style="text-align:left">
        <p>0.002&quot;-0.005&quot; /</p>
        <p>0.05mm-0.13mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;-0.0025&quot; /</p>
        <p>0.025mm-0.063mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot;-0.001&quot;/</p>
        <p>0.013mm-0.025mm</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">1/4&quot;</td>
      <td style="text-align:left">
        <p>0.002&quot;-0.01&quot; /</p>
        <p>0.05mm-0.254mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;-0.005&quot; /</p>
        <p>0.025mm-0.127 mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;-0.002&quot;/</p>
        <p>0.025mm-0.05mm</p>
      </td>
    </tr>
  </tbody>
</table>Now we have to take a little detour and talk about stepover, because it impacts the _effective_ chipload.

## Stepover/WOC/RDOC

"**Stepover**" refers to the offset distance between one cutting pass and the next one, which also translates into how much new material is being removed by the endmill, or how much radial engagement is put on the endmill. It's also called Width of Cut \(**WOC**\) or sometimes Radial Depth of Cut \(**RDOC**\)

In the example below, the stepover S if 50% of the endmill diameter:

![](.gitbook/assets/stepover.png)

The larger the stepover, the larger the force on the endmill. Cutting passes with a small stepover are better for surface finish quality, while passes with large stepover obviously reduce overall cutting time since less passes are required to cut a given amount of material.

For Ball-end endmills, stepover value influences surface finish quite a lot. Consider the following sketch of a side view showing multiple passes:

![](.gitbook/assets/stepover_ballend_scallop.png)

Due to the geometry of the endmill tip, scallops of residual material will be left at regular interval on the bottom surface. These will be more or less visible depending on how well the material can hold small details. 

* a 20% to 33% stepover should be small enough for wood.
* a 10 to 20% stepover should work for metal.

## Chip thinning

The chipload values discussed above assume that the cutter circumference is engaged deep enough into the stock, that each flute gets to cut material from the moment it starts biting \(at the bottom of the blue circle\) to the moment it completes the 90° rotation of its useful activity for that revolution. This happens when the stepover is at least 50% of the endmill diameter:

![](.gitbook/assets/chip_thinning1.png)

Now consider what happens if the stepover is lower than 50% of the diameter, say 20% only:

![](.gitbook/assets/chip_thinning2.png)

For the same RPM and feedrate, the _actual_ size of each chip is smaller, because the flute exits the material early and the rest of its 90° of useful action is not used. The effective thickness of the chip is lower than expected, which also means that less heat is removed, so this is not optimal.

The solution is to compensate this effect by **increasing the feedrate** \(all other parameters staying the same\), such that the size of the chip is increased to approximately the same amount of material that _would_ have been removed if the cutter was engaged at 50% of its diameter:

![](.gitbook/assets/chip_thinning3.png)

The size of the adjusted target chipload is determined by:

$$
Chipload(adjusted) = \frac{Diameter}{2 * \sqrt{(Diameter * Stepover) - Stepover^2}}* Chipload
$$

{% hint style="info" %}
For basic toolpaths, the stepover is often in the 40% to 50% range, and it's possible to just ignore chip thinning altogether. Where chip thinning really matters is for adaptive clearing toolpaths, that typically use small stepovers \(more on this in the [Toolpaths](toolpath-basics.md#adaptive-clearing-toolpaths) section\)
{% endhint %}

## Choosing RPM and Feedrate

At this stage, the material is known, the endmill geometry is known, chip thinning is accounted for, which gave us an adjusted target minimal chipload. The remaining part is to chose a specific combination of RPM and feedrate values that together will produce this chipload, following the formula described earlier.

In theory, there are two options: selecting a feedrate value and solving for the associated required RPM value, or selecting an RPM value and solving for the associated feedrate.

In practice, the latter is done. The main reason is that the traditional way to determine feeds and speeds \(especially when cutting metal\) is to start from the required **SFM** \(Surface Feet Per Minute\): this is the linear speed of the edge of the cutter, and it should within a certain range depending on the material and the endmill. And to achieve a given SFM for a given endmill diameter, only the RPM needs to be determined:

$$
RPM = \frac{SFM(in \ feet\ per\ min)}{0.262 * endmill\_diameter(in\ inches)}
$$

In practice, for most of the materials cut on a Shapeoko, there is a wide range of acceptable SFM, so RPM could initially be chosen pretty much anywhere within the router's RPM limits \(10k to 30k for the Makita/Carbide router, 16k to 27k for the Dewalt router, and typically a few hundred to several tens of kRPM for spindles\)

* Low RPMs are quieter \(significantly so with a router\), but induce higher forces on the cutter \(more on this later\)
* High RPMs induce lower cutting forces and generally provide better finish quality, but will also require faster associated feedrates to maintain a correct chipload: feeding faster can be a little scary at first, and leaves less time to react should anything go wrong.

A rule of thumb is therefore to set RPM to "**the maximum value you can tolerate and be confortable operating at**", and determine the associated feedrate to get the target chipload.

Example:

* Material is hard wood and endmill is a 3-flute 1/4" =&gt; the recommanded chipload table recommands up to 0.002"
* Say we use a 25% radial depth of cut / stepover, i.e. 25% of 50% of 1/4" = 0.03125, so adjusted chipload is: 

$$
\frac{0.25}{2 * \sqrt{(0.25* 0.03125) - 0.03125^2}}* 0.0014" \approx 1.5*0.002" = 0.003"
$$

* The ideal setting would be to max out the RPM, say 24.000 \(to take an example that is reachable on the Makita, DeWalt, and common spindles\). The required feedrate would then be :

$$
Feedrate = Chipload*Nb\_Flutes * RPM= 0.003 * 3 * 24000 = 216\ ipm
$$

* That is above the capability of the Shapeoko \(200ipm\), it would be scaringly fast for cutting hard wood, and 24.000 RPM may sound too loud to your taste anyway. Let's say we decided to go for 16.000 RPM instead,  the required feedrate would become:

$$
0.003 * 3 * 16000 = 144\ ipm
$$

* If going 144ipm still _feels_ a little fast, it possible to obtain the same chipload at lower RPM and lower feedrate, e.g. 12.000RPM and 108ipm, at the expense of higher cutting forces \(which or may not be a problem, see power analysis section later below\)
* Alternatively it is also possible to lower the feedrate by targetting a smaller chipload while ensuring it is still at least at the minimum recommanded of 0.001", and assuming you are using a sharp enough cutter: 
  * To get a 0.001" effective target chipload, the adjusted target chipload would become 0.002"
  * the feedrate would then be 0.0015 \* 3 \* 16.000 = 72ipm 

{% hint style="info" %}
Some usecases call for the use a O-flute endmills: this will probably mean reducing the feedrate and/or increasing the RPM to maintain a proper chipload.
{% endhint %}

## Choosing DOC & WOC

**Depth of Cut** \(DOC\) a.k.a. Axial Depth of Cut \(ADOC\) a.k.a. Depth Per Pass, is how deep into the material the endmill will be, along the Z axis. For a given feedrate and RPM, the deeper it is the larger the forces on the endmill.

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

Check out the **adaptive clearing** strategy in the [Toolpaths](toolpath-basics.md) section, for how to address this. With adaptive clearing, depth of cut of several times the endmill diameter are reachable.
{% endhint %}

TODO WOC



A common rule of thumb is to use:

* **35% to 50%** stepover for roughing using regular toolpath
* **10% to 35%** stepover for roughing using adaptive clearing toolpath 
* **5% to 10%** stepover for finishing

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

## Plunge rate

All of the info above only focused on the feeds and speeds for the radial part of the cut, but when the endmill is plunging \(straight down/vertically\), things are quite different:

* \(obviously\) the cutting edges on the circumference of the endmill are not cutting anything anymore, the cutting happens at the tip of the endmill only, like a drillbit.
* But endmills are really not optimized for drilling, so their ability to plunge efficiently through material is quite limited

One could compute specific plunge rate and RPM based on the specific geometry of the tip of the endmill, but in practice it's easier to just:

* use the same RPM as for radial cuts. This is a given when using a router where there is no dynamic control on the RPM anyway, so the same value is used throughout the cut. It would not make much sense either on a spindle to accelerate/decelerate the RPM
* use a plungerate that is experimentally chosen, following the rule of thumb 
  * **10% to 20%** of the feedrate for metals
  * **30% to 40%** of the feedrate for woods 
  * **40% to 50%** of the feedrate for plastics \(plunging fast is required to avoid melting\)

{% hint style="info" %}
These numbers are for plunging straight down. If the toolpath uses some ramping at an angle into the material, they can be increased quite a bit.
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
  * It is **100%** \(by definition\) when slotting
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
  * **5% to 10%** of the diameter of the endmill for regular toolpath in metals e.g. **aluminium**
  * **10% to 50%** of the diameter of the endmill for regular toolpath in **softer materials**
  * Basically **up to the full length of cut** of the endmill for adaptive toolpaths or finishing passes, but more likely **200%** of the endmill diameter
* if possible, check **deflection** to make sure there is no risk to break the tool

Then **experiment**: run a cut with those parameters, and incrementally increase chipload \(i.e. feedrate, if keeping RPM constant\) and/or DOC, until chatter or poor finish quality occurs, then dial back feedrate and DOC by 10% and you should be in the sweet spot!

## Telltale signs of wrong F&S

The most common signs of inadequate feeds and speeds are:

* sound, and specifically **chatter**: when feeds and speeds are not right for a given material/endmill/DOC, the tool tends to vibrate, and this vibration can get worse if there is resonance with another source of periodic variation elsewhere in the system \(most often: the router and its RPM\). This results in an ugly sound, a poor finish with marks/dents/ripples on the surface, and a reduced tool life.
* **finish quality**: even without chatter, a poor surface finish can indicate that the final cutting pass was too agressive \(high chipload\)
* **melted material**: especially in plastics and soft metals like aluminium, if the feedrate is too low for the selected RPM, the friction will cause the material to melt rather than shear, the tool flutes will start filling with melting material, and this usually ends up with tool breakage.
* **endmill temperature**: the endmill just not be more than slightly warm at the end of a cut: if it gets hot to the touch, the feeds and speeds are likely not correct. In extreme cases, the endmill color itself may change to a dark shade.
* making **dust**, instead of clearly formed chips \(MDF is an exception, you just cannot get chips anyway with this material\)

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
Cutter\ power\ (in\ HP\ unit)= \frac{MRR}{Material\ K\ factor}
$$

the "K" factor is a constant that depends on the material's hardness, and corresponds to how many cubic inches per minute \(or cubic millimeters per minute\) of material can be removed using 1 horsepower.

For example, 

* 6061 T6 aluminium has a K of 3.34 cubic inches per minute
* it's about 10 in3/min for hard woods ahd hard plastics
* and up to 30 in3/min for soft woods, MDF, ...

Once you get this power value, you can compare it to your router's maximum power, which is rated at a max of 1.25HP \(932Watts\), but that is power input, and the power efficiency of a router is not very good \(~50%\) so the max actual power at the cutter is more likely around 450W.

And finally, even if the cutting power is within the range of your router, there is still the matter of the **cutting force** that the Shapeoko has to put on the X/Y/Z mechanical axis to move the endmill through the material: 

&lt;TODO sketch force around a circular movement&gt;

$$
Cutting\ torque (in\ N.m)= \frac{Cutter Power (Watts) }{2\pi *RPM/\ 60}
$$

or in the Imperial units converting N.m to lbf-in \(x8.85 factor\) , since \(60 \* 8.85\) / \(2 \* 3.14159\) = 84.5:

$$
Cutting\ torque (lbf\_in)= \frac{Cutter Power (Watts) * 84.5}{RPM}
$$

and finally cutting force is:

$$
Cutting\ force= \frac{Cutting\ torque}{endmill\ radius}
$$

which can be compared with the Shapeoko's limit estimated around **20 lbf** \(9Kg\)

## Calculators

This section should have highlighted that MANY factors influence the selection of adequate feeds & speeds & DOC settings. 

So at some point, it becomes impossible to produce feeds & speeds charts for every possible combination of factors, and also very difficult to compute everything manually. A number of calculators have been implemented to address this, ranging from free excel spreadsheets that basically implement the equations mentioned above, to full-fledged commercial software that embbed material/tool databases, the most famous one probably being **G-Wizard.**

Whether or not you _need_ a feeds & speeds calculator is debatable: most people use a limited number of combinations of material/endmill sizes anyway, in which case relying on a few good feeds & speeds & DOC recipes for your machine is enough. The real value of calculators is in **optimizing** the feeds & speeds to achieve a better material removal rate or finish quality \(by keeping deflection under control\) or tool life, etc...if this is your thing.

