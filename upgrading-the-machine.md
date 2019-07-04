# Upgrades

The Shapeoko is a great machine out of the box, and there is really no _need_ to modify it to get good results. No, really, check out Winston Moy's videos if you need convincing. Still, once you are reasonably comfortable with the machine, and how to set optimal CAD/CAM parameters, chances are you may be tempted to optimize the machine itself. 

There are many possible hardware upgrades. Some are cheap, some are definitely expensive: whether they add enough value for money is debatable and depends completely on your usecases.

## Proximity switches

The standard limit switches are fine, since they have mechanical parts they can wear out over time and eventually fail, which would result in a machine crash when homing. But the main reason to upgrade is to get an \(even\) better precision/repeatability when homing. 

**Contactless/proximity switches** trigger when a metal object comes within a certain distance of their surface. Since there is no mechanical contact with the moving part, there is no mechanical wearout, and they also happen to be more precise. Here's an example of a proximity switch for the Z-axis:

![](.gitbook/assets/upgrades_proximity_switch.png)

They can be used as an \(almost\) drop-in replacement for the orignal switches, the only difference is that \(depending on their technology\) they may need an additional lead for power supply, connected to one power pin of the controller board \(typically, the 5V pin on the Arduino ISP header, see [Anatomy of a Shapeoko](anatomy-of-a-shapeoko.md#controller-board) for details\)

## X/Z axis upgrade

Probably the most popular upgrade is the replacing the stock X/Z carriage with a sturdier one. This is by nature a costly upgrade since it involves multiple large and sturdy parts, linear rails, and a likely ball screw. A few reasons to consider this upgrade path are:

* to eliminate the chances of the Z-belt slipping on the pulley when improperly tensioned
* to eliminate the intrinsic \(small\) front/back slop of the original design, between the main Z-plate and the sliding plate
* to support the weight of a spindle, that is typically much heavier than a router. Additional springs can be added as an alternative.

{% hint style="info" %}
One noticeable difference between a ball screw driven X/Z axis and a belt & springs one, is that when power is removed from the stepper the router/spindle will stay at its current Z \(whereas with it drops naturally with belt & springs\). This is usually a good thing \(no unintended drop\) but sometimes a minor nuisance \(Z cannot easily be moved manually while machine is turned off\) 
{% endhint %}

&lt;TODO photo of the HDZ&gt;

{% hint style="warning" %}
Since alternative X/Z solutions have different Z-travel capacity than the stock X/Z carriage, you will need to adjust a few parameters in GRBL accordingly. And if you are using Carbide Motion as a G-code sender, you _may_ need to migrate to another sender that does not make any hardcoded assumptions about the machine geometry
{% endhint %}

## Bed upgrade

There are two main reasons to replace the original MDF bed:

* to match a specific workholding solution \(e.g. T-tracks\)
* to make the Shapeoko more rigid

While the T-tracks bed upgrade \(that comes as an alternative to the "sea of holes" wasteboard\) typically still uses MDF strips, upgrading to an aluminium bed goes a long way to improve machine rigidity as well as have a more "weather-insensitive" machine \(MDF tends to absorb humidity, which _may_ lead to some warping/swelling over time\)

![](.gitbook/assets/upgrades_aluminum_bed.png)

This one is 12mm / ~0.5", the one on Carbide3D's store is 0.5" thick too.

{% hint style="info" %}
This is still only a "nice to have" and expensive upgrade: the large majority of Shapeoko users have an MDF bed and are happy with it
{% endhint %}

## Belts upgrade

This is probably the cheapest upgrade: replacing the original GT2 belts with reinforced ones. The two most popular are **steel-core** belts, and **kevlar** belts.

Here's a section view of steel-core belts showing the embedded steel wires:

![](.gitbook/assets/upgrades_steel_core_belt.png)

Buying several meters of steel-core belts is cheap, will serve as a provision in case the original belts snap, and replacing the belts is very easy \(since it does not involve disassembling anything else that the belt tensioners\) so that upgrade is a no brainer if you feel you can benefit from the increased robustness, and possibly from the ability to push the max speed and acceleration beyond the default settings.

![](.gitbook/assets/upgrades_steel_core_belt_mounted.png)

## Automatic \(router\) RPM control

Having to manually set RPMs on the router knob at the beginning of each job can get old, and is also error prone. Luckily the Shapeoko controller board happens to have a "PWM" output that GRBL modulates as a function of the RPM values found in the G-code. So it is possible to feed this signal into a dedicated power controller, that will adjust the voltage applied on the router power leads accordingly, resulting in a specific RPM value.

This requires to buy such a power control module, the most popular one is the **SuperPID**. You need to know what you are doing since installing it requires wiring mains. 

Beyond the automatic RPM control, the added benefit is that this allows the router to operate at a lower RPM value than the minimal knob setting, e.g. to run at 5000 RPM on a Makita that normally mins out at 10.000RPM. 

## Spindle upgrade

The Shapeoko uses a trim router by default for cost/convenience reasons, but all higher-end CNCs use a spindle instead. Some of the main benefits are:

* a spindle can run at lower RPMs than a router \(some models can run at much _higher_ max RPMs too\), and usually has higher torque. Since a spindle needs a dedicated controller to run anyway, the automatic RPM control described above is a given.
* the runout is smaller/better
* it is waaaay quieter than a router
* there is no need to change bushing, basically no maintenance.
* it usually supports "ER" collets, which come in much more varied sizes than trim router collets.

They come in two types: air-cooled and water-cooled \(you then need to add a water tank & pump in the setup\), and at various power ratings, the most popular being 1.5kW and 2.2kW.

They are significantly heavier than a router, so this upgrade is often associated to a ball screw Z axis upgrade. 

Installation will require to wire the PWM signal from the Shapeoko controller board to the spindle controller. The wiring/configuration of the spindle controller itself may not be a piece of cake, depending on the available documentation \(spoiler alert: chances are you will buy your spindle kit in China, documentation will be sub-par, but the community is here to sort it out\)



