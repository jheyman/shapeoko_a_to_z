# Feeds & speeds

{% hint style="info" %}
Disclaimer/TL;DR: 

There are so many factors involved in figuring THE perfect feeds and speeds values for a given usecase, that it is easy to either:

* think it is so complicated it looks like black magic, and proceed to determine proper values experimentally instead \(which is perfectly reasonable!\)
* think the description below is over-simplified because it does not take into account &lt;some additional obscure parameter&gt;

This section is intended for curious users who want to understand where all the magic values come from, as well as have a deterministic way to compute reasonable feeds and speeds starting point for their situation.

The conclusion is "get a feeds & speeds calculator, but understand what it does for you".
{% endhint %}

"**Feeds**" is feedrate, i.e. the speed of the gantry pushing the endmill into the material

"**Speeds**" is the rotation speed of the endmill, i.e. RPM.

The reason these words are used in conjunction is that their values are tightly coupled, so what matters is the _combination_ of feedrate and RPM for a given situation.

Using a proper "Feeds and speeds" combination is the key to optimize:

* material removal rate \(e.g. how long it takes to cut that 1 inch pocket\)
* quality of the cut \(surface finish\)
* tool life \(i.e. keep tool wear to a minimum\)
* avoid/minimize chatter \(the horrendouse sound heard when the cutting parameters are such that the endmill/machine vibrates while doing through the stock material\)

&lt;TODO : intro sketch showing all the inter-dependent factors + comment that sections below propose a way to approach solving the equation&gt;

Before diving into the relation between feedrate and RPM, let's check how the tool cuts the stock material.

## Conventional milling

In so-called "conventional" milling, the direction of the endmill movement is such that the cutting teeth bite from the inside to the outside of the material. In the sketch below, imagine the blue triangle represents one cutting tooth of the endmill. If the blue cross is the position of the center of the endmill when this tooth starts biting into the material, and the endmill is moving into the material at feedrate F, then a little bit later the endmill center is at the position of the purple cross, and the cutting tooth has rotated and gone out of the material. The resulting chip of material that was cut during that time is the green part.

It starts out very thin, and gradually increasing in thickness. The maximum thickness is when the cutting tooth exits the material, noted "C" below, is typically called the "feed per tooth" or "**chipload per tooth**".

![](../.gitbook/assets/chipcut_conventional.png)

## Climb milling

"Climb" milling is when the direction of the endmill movement is such that the cutting teeth bite from the outside to the inside of the material. In this situation, the cutting tooth first bites a large chunk of material \(blue position\), and as the endmill rotates and moves to the left at feedrate F, the cut gets thinner, until the tooth has nothing left to cut \(purple position\). The resulting chip \(in green\) has a similar shape than in conventional milling, and again the max thickness of the chip is the chipload.

![](../.gitbook/assets/chipcut_climb.png)

Climb milling is harder on the machine, so it requires a good rigidity \(which is somewhat limited on a stock Shapeoko, and is what it is...\) and strong workholding \(which is the part one can do something about, using a proper clamping system for example\)

## Chipload vs. Feeds and Speeds

So why does chipload per tooth matter ? 

A large chipload requires a lot of force/power/rigidity from the machine \(i.e. there's always a limit to the size of the bite you can take, whether you're a squirrel or a white shark\)

A \(too\) small chipload is not good either, because while cutting through the material the endmill heats up, and this heat needs to go somewhere. The chip carries some of the heat as it flies away, and the amount of heat it removes is linked to its size. So if the chip is too small, most of the heat stays on the endmill, which reduces tool life and the quality of the cut. Also, if the chipload gets really small, and since the cutting teeth are not infinitely sharp, instead of slicing into the material the tooth will just rub against it, and then "heat happens".

So again this is a goldilocks situation: the chipload must be high enough to avoid rubbing and overheating of the endmill, and small enough to be compatible with the power/rigidity limits of the machine.

Endmills often have several cutting teeth, and each one contributes in turn to removing material during one revolution of the tool. If we sketched the successive bites that the endmill takes into the material, this would look something like this:

![](../.gitbook/assets/chipload_per_revolution800.png)

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

## Chipload versus endmill size & material

Optimal chipload depends on a lot of things, but mostly:

* the \(hardness of the\) **material** being cut
* the **diameter** of the endmill \(smaller teeth need to take smaller bites\)
* the **rigidity** of the machine: it is quite easy to forget that the Shapeoko is a hobbyist-grade CNC and is not as rigid as the professional CNCs, so general recommanded chiploads may not be suitable for the Shapeoko.

This last element is what makes optimal chipload a bit difficult to predict: not only is the Shapeoko different \(less rigid\) than pro CNCs, there can also be differences in rigidity between two Shapeokos depending on how they were assembled/setup, whether they have any mod, etc...

So it is almost impossible to come up with a rule that will provide the _optimal_ chipload for all machine & usecases directly. What is possible though, is to provide a guideline as to the _minimal_ reasonable chipload that can be used as a starting point, and a process to optimize it on a given machine for a given usecase .

If you look around the web for recommanded chipload values and average them out, you may find something along the lines of: 

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">MDF</th>
      <th style="text-align:left">Soft wood</th>
      <th style="text-align:left">Hard wood</th>
      <th style="text-align:left">Acrylic</th>
      <th style="text-align:left">Aluminium</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1/16&quot;</td>
      <td style="text-align:left">
        <p>0.0007&quot;/</p>
        <p>0.0178mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0007&quot; /</p>
        <p>0.0178mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0006&quot; /</p>
        <p>0.0152mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot;/</p>
        <p>0.0127mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot; /</p>
        <p>0.0127mm</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">1/8&quot;</td>
      <td style="text-align:left">
        <p>0.0055&quot;/</p>
        <p>0.1397mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0045&quot;/</p>
        <p>0.1143mm</p>
      </td>
      <td style="text-align:left">
        <p>0.004&quot;/</p>
        <p>0.1016mm</p>
      </td>
      <td style="text-align:left">
        <p>0.003&quot; /</p>
        <p>0.0762mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0035&quot;/</p>
        <p>0.0889mm</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">1/4&quot;</td>
      <td style="text-align:left">
        <p>0.0145&quot;/</p>
        <p>0.3686mm</p>
      </td>
      <td style="text-align:left">
        <p>0.012&quot;/</p>
        <p>0.3048mm</p>
      </td>
      <td style="text-align:left">
        <p>0.01&quot;/</p>
        <p>0.254mm</p>
      </td>
      <td style="text-align:left">
        <p>0.008&quot;/</p>
        <p>0.2032mm</p>
      </td>
      <td style="text-align:left">
        <p>0.006&quot;/</p>
        <p>0.1524mm</p>
      </td>
    </tr>
  </tbody>
</table>but these are mostly from endmill manufacturers, who assume you are using their endmill on a pro machine. For the Shapeoko we need to dial back these chiploads. There is no scientific rule to do this, so let's use a few \(over-\)simplification assumptions:

* chipload scales linearly with endmill diameter
* a commonly seen value for cutting aluminium on a Shapeoko is 0.001" for a 1/4" endmill
* chipload can be increased for softer materials, let's pick a rough estimate of +25% when going from harder to softer among aluminium, acrylic \(hard plastics\), hard wood, soft wood, and finally MDF.

If we apply this recipe, we end-up with this guideline for the minimal useful chiploads on the Shapeoko:

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">MDF</th>
      <th style="text-align:left">Soft wood</th>
      <th style="text-align:left">Hard wood</th>
      <th style="text-align:left">Acrylic</th>
      <th style="text-align:left">Aluminium</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">(soft wood + 25%)</td>
      <td style="text-align:left">(hardwood + 25%)</td>
      <td style="text-align:left">(acrylic + 25%)</td>
      <td style="text-align:left">(aluminium + 25%)</td>
      <td style="text-align:left">(baseline)</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">1/16&quot;</td>
      <td style="text-align:left">
        <p>0.0006&quot;/</p>
        <p>0.0152mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot;/</p>
        <p>0.0127mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0004&quot;/</p>
        <p>0.0102mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0003&quot;/</p>
        <p>0.0076mm</p>
      </td>
      <td style="text-align:left">
        <p>0.00025&quot;/</p>
        <p>0.0063mm</p>
      </td>
      <td style="text-align:left">(half of 1/8&quot; values)</td>
    </tr>
    <tr>
      <td style="text-align:left">1/8&quot;</td>
      <td style="text-align:left">
        <p>0.0012&quot;/</p>
        <p>0.0305mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;/</p>
        <p>0.0254mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0008&quot;/</p>
        <p>0.0203mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0007&quot;/</p>
        <p>0.0178mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&quot;/</p>
        <p>0.0127mm</p>
      </td>
      <td style="text-align:left">(half of 1/4&quot; values)</td>
    </tr>
    <tr>
      <td style="text-align:left">1/4&quot;</td>
      <td style="text-align:left">
        <p>0.0025&quot;/</p>
        <p>0.0635mm</p>
      </td>
      <td style="text-align:left">
        <p>0.002&quot;/</p>
        <p>0.0508mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0015&quot;/</p>
        <p>0.0381mm</p>
      </td>
      <td style="text-align:left">
        <p>0.00125&quot;/</p>
        <p>0.0318mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&quot;/</p>
        <p>0.0254mm</p>
      </td>
      <td style="text-align:left">(baseline )</td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
For reference, the absolute maximum chipload that a stock Shapeoko can theroetically achieve is when using the fastest feedrate \(100"/min\), the slowest RPM \(12.000 RPM on the Dewalt router\), and the lowest number of flute \(1\) : max\_chipload =  100 / \(12000\) = 0.083"

The max chipload that can be achieved when using the most common endmill \(3-flute 1/4", e.g. Carbide3D's \#201\) is 100/ \(3x12000\) = 0.0028". So that value for MDF using a 1/4" endmill sounds about right.
{% endhint %}

&lt;TODO: add snapshot of "good" chip versus dust + add hint, and more generally real life experiment results&gt;

## Radial width of cut / Stepover

"**Stepover**" refers to the offset distance between one cutting pass and the next one, which also translates into how much new material is being removed by the side of the endmill, or how much radial engagement is put on the endmill:

&lt;TODO sketch to explain stepover : example pocketing toolpath or based on convent milling sketch above/engagement &gt;



BALL:

&lt;TODO sketch scallop versus stepover for ball endmill&gt;

Typical values : 10 to 33% of the endmill diameter

* 10 to 20% stepover for metals
* 20% to 33% for wood

SQUARE:

&lt;TODO sketch : no scallop&gt;

no scallops but tool load/deflection

35%-40% for roughing, 5% for finishing.



&lt;TODO check usual stepover in real life examples&gt;



## Chip thinning

We're almost there, but there is one more thing: the above-mentioned chipload values assume that the cutter circumference is engaged deep enough into the stock, that each tooth gets to cut material from the moment it starts biting \(at the bottom of the blue circle\) to the moment it completes the 90° rotation of its useful activity for that revolution. This happens when the cutter's **radial depth of cut \(stepover\)** is 50% of its diameter:

![](../.gitbook/assets/chip_thinning1.png)

Now consider what happens if the radial depth of cut is lower than 50% of the diameter, say 20% only:

&lt;TODO rescan \(transparency\)&gt;

![](../.gitbook/assets/chip_thinning2.png)

For the same RPM and feedrate, the _actual_ size of the chip that each tooth produces is smaller, because the tooth exits the material early and the rest of its 90° of useful action is not used. The thickness of the chip is lower than expected, which also means that less heat is removed, so this is not optimal.

The solution is to compensate this effect by **increasing the feedrate** \(all other parameters staying the same\), such that the size of the chip is increased to approximately the same amount of material that would have been removed if the cutter was engaged at 50% of its diameter:

![](../.gitbook/assets/chip_thinning3.png)

The size of the adjusted target chipload is determined by:

$$
Chipload(adjusted) = \frac{Diameter}{2 * \sqrt{(Diameter * RadialDOC) - RadialDOC^2}}* Chipload
$$

{% hint style="info" %}
In many situations, it's possible to just ignore the chip thinning effect and proceed with a recommanded chipload value. But it's easy enough to use a feeds & speeds calculator that does take it into account, more on this below.
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

**Depth of Cut** \(DOC\) also called Depth Per Pass, is just as important as feeds & speeds yet surprisingly there is much less information about how to determine its value, compared to the abundance of feeds and speeds chart.

&lt;TODO sketch montrant depth per pass + total depth&gt;

The reason is probably that while there are mathematical recipes to choose feedrate and RPM for a given endmill geometry, the achievable DOC is much more tightly linked to the specific machine you are using, and specifically its rigidity and power.

&lt;sketch force proportional to engaged flute length&gt;

The Shapeoko is not as rigid nor as powerful as pro CNC machines, so DOC recommandations for these machines need to be dialed back, even when using perfect feeds and speeds.

A good rule of thumb is to _start_ with a DOC that is ****around: 

* **5% to 10%** of the diameter of the endmill for metals e.g. **aluminium**
* **10% to 50%** of the diameter of the endmill for **softer materials**

If it cuts fine, then increase DOC gradually to find the limit for your machine/endmill/material.

&lt;TODO sketch showing only a little fraction of the cutter being used + link to adaptive&gt;

{% hint style="info" %}
There are two direct implications of having to limit the DOC to relatively small values:

* cutting deep into material will require many passes, hence of lot of time.
* during each cutting pass, only the tip of the endmill is used, so this part will wear out over time, while the rest of the flute length is never used.

Check out the **adaptive clearing** **toolpath** described later in the book, for how to address these two limitations.
{% endhint %}

## Slotting

You may have noticed that all sketches above only consider cases where only one side of the endmill is cutting into the material. Depending on the stepover, the Tool Engagement Angle \(TEA\) is anywhere between a few % and 90%:

&lt;TODO sketch of 45% and 90% TEA&gt;

**Slotting** is a different story: both sides of the endmill are engaged, so the TEA is 180°:

&lt;TODO sketch of 180° engagement&gt;

The force on the endmill will be twice as much as when cutting at 90° TEA, so the max achievable chipload/DOC combination for a given machine/endmill/material is lower. The recommanded chipload/DOC values above include some margin to take this effect into account to some extent.

The other side effect of slotting is that chip evacuation is not as good: the flutes are in the air only 50% of the time, so the chips that form inside the flute have less time/opportunities to fly away.  Deep slotting is notorious for causing issues when chips cannot be evacuated quickly enough.

Bottomline: when slotting you may have to:

* limit DOC to a smaller value
* limit feedrate \(chipload...\)

{% hint style="info" %}
Or you can take a different approach and _avoid_ slotting altogether, by using smarter toolpaths. See adaptive clearing and pocketing in the toolpath section
{% endhint %}

## Corners

While we are talking about TEA, let's take a look at what happens when cutting a square pocket at 50% tool engagement \(90° TEA\) and changing direction at a corner:

&lt;TODO 90° / 180° tool engagement&gt;

While cutting the corner, the TEA momentarily goes up to 180°, same as during a slotting cut, which has the same detrimental effects!

Similarly to slotting, the cure is to dial back feedrate and DOC. 

{% hint style="info" %}
But that boils down to optimizing the cut parameters used throughout the job, for these very short times when the corners are being cut, which is not very efficient. The alternative include avoiding straight corners in the design if possible \(e.g. round the corners...\) or use adaptive clearing toolpath that will take a lot of very shallow bites at the corner instead of a deep one.
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
These numbers are for plunging straight down. If the toolpath uses some kind of ramping at an angle into the material, then they can be increased quite a bit.
{% endhint %}

## Wrapping up

A possible workflow is therefore:

* select **material**: whatever the specific project calls for.
* select an **endmill type and size**: usually directly linked to the size of the smallest feature that will be need to be cut
* look-up **desired minimum chipload** for this material+endmill combination
* choose a radial depth of cut / stepover, and **adjust target chipload for chip thinning**
* select **target RPM** \(e.g. based on SFM for metal, or another criteria, I like quietness\)
* determine the specific **required feedrate** for this RPM to achieve the adjusted chipload
* determine **plungerate** \(10 to 50% of feedrate depending on material hardness\)
* determine initial **depth of cut**: 10 to 50% of endmill diameter depending on material
* **experiment**: run a cut with those parameters, and then incrementally increase chipload \(feedrate\) and/or DOC, until chatter or poor finish quality occurs, then dial back feedrate and DOC by 10% and you should be in the sweet spot!

## Calculators

This section should have highlighted that MANY factors influence the selection of adequate feeds & speeds & DOC settings. But wait, there are more. For example, **tool deflection** needs to be taken into account:

&lt;TODO sketch tool deflection&gt;

So at some point, it becomes impossible to produce feeds & speeds charts for every possible combination of factors. A number of F&S calculators have been implemented to address this problem, ranging from free excel spreadsheet that basically implement the equation mentioned above, to full-fledged commercial software, the most famous one probably being G-Wizard.

Whether or not you _need_ a feeds & speeds calculator is debatable: most people use a limited number of combinations of material/endmill sizes anyway, in which case relying on a few good feeds & speeds & DOC recipes for your mahcine is enough. The value of calculators is in estimating the correct settings for that NEW combination that you never tried, but above all to **optimize** the feeds & speeds to achieve a better material removal rate or finish quality.



## Effects of incorrect feeds and speeds

chatter, finish quality, heating/rubbing =&gt; sound, sound,sound.



## Experimental optimization

TODO: experimental method \(cf PreciseBits\)

TODO : propose test pattern and G-code \(+ G-code generators ?\)









