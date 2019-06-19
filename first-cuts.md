# Running a job

The basic workflow for running a job is covered pretty well in Carbide3D's docs/tutorials, the intent of this section is to provide background information and various tips about the different steps. 

{% hint style="info" %}
The ordering of these steps can vary based on personal preference and software being used. For example, I like to have the machine turned off completely while I change the tool, but many find it more convenient to have it on to be able to jog the router automatically to the front.
{% endhint %}

## Installing the tool

A few things to watch out for when installing a tool in the collet :

* make sure the collet matches the endmill diameter. This may sound silly, but once you start having a collection of various imperial and metric tools and collets, inadvertently using a 6mm endmill in a 1/4" \(6.35mm\) collet is a possibility, and not paying attention to this will result in serious trouble during the cut. 
* the collet and especially the taper must be **clean** of any debris/dust. Those might introduce runout 
* **stickout**: it should be 
  * as low as possible to minimize tool deflection
  * but still be compatible with the max cutting depth for this job
  * short enough that the shank is engaged over the full height of the collet
  * but not pushed so far back that the flutes are in the collet
* **runout**: if it matters for the job at end \(high-precision and/or micro-machining\), now is the time to check it and tune it.
* the saying goes that the collet should be "**monkey tight, but not gorilla tight**". It should not be a baby monkey either, or the endmill might slip in the collet, and bad things will happen.

## Moving manually

Whenever the machine is turned off, it is possible to move the machine manually, with just one contraint: do it slowly. When the machine is off and is moved manually, the stepper motors behave as alternators, they generate current, enough current in fact to back-power the electronics \(you might see the LED on the controller board come to life...\). Moving slowly ensures that the generated current stays low enough to not  damage the stepper electronics. 

## Homing

The reason for homing was covered in the [Coordinate system]() section: the only way the Shapeoko can tell for sure where it is, is when it contacts the three limit switches. Without homing, each time the machine is power cycled it would then be unable to go back to any specific coordinates, e.g. if the rails have been moved manually in the meantime.

You could argue that homing is useless since you are going to manually jog to the Zero point and set it to be the reference anyway. For a job that can be done in a single run / with a single tool, that could work. but the power of homing is that it will allow to return with great precision to Zero point defined last \(which happens to be stored in the non-volatile memory of the controller, by the GRBL software\). 

If you turn off the machine to change tools, this will be needed. But even if you do not turn off the machine to change tools, you may still want to home between runs: if for some reason the steppers or belts skipped a few steps during the previous run, returning to Zero without homing first _may_ bring the machine to a subtly different point, a few steps away. If you are doing a 2-sided job or anything that requires precision and uses successive passes with different tools, these few steps may matter.

## Setting & checking RPM

Unless you have installed a spindle or a PID controller \(see [Upgrading the machine](upgrading-the-machine.md)\), you need to adjust RPM manually using the knob on the trim router. 

The [Anatomy of a Shapeoko](anatomy-of-a-shapeoko.md#trim-router) section has a reminder about the mapping between knob values and actual RPM, but the actual RPM can be slightly different than advertised and it's not easy to interpolate between knob settings to use intermediate RPM values, so I find it much easier to buy a laser tachymeter \(about 20$\), turn the router on, and adjust the know to get the desired value:

 &lt;TODO include blog pic of tachymeter&gt;

## Zeroing \(manually\)

After jogging to the vertical of the intended zero point on the stock surface, lower Z gradually then use fine steps to touch off on e.g. a piece of paper placed between the tool and the stock, stopping as soon as the paper cannot be moved freely. Yes, it does mean that you will zero a tiny bit above the real stock surface, but the average thickness of a piece of paper is around 0.004" / 0.1mm, and chances are that you are already compressing the paper since it cannot move anymore, so you will actually be very, very close to stock surface.

*  Double-check your Z jog step before doing the final steps to touch off. It is very easy to hit on the "down" arrow one time too many by mistake, and if the step is still say 0.1", this might bury your tool into the stock, or more likely break it if it is a small endmill.
* Touching off with a very pointy V-bit can be tricky, it's easy to go too far down without noticing, so you should use even finer jog steps and/or extra caution.
* TODO check CC 4.0 behavior + mention alternate senders.

## Zeroing \(with a Probe\)

With a touch probe, the touch off process can be automated. The Shapeoko controller has a dedicated "Probe" input, that works like the other limit switches: it detects whether there is continuity between the two pins:

![](.gitbook/assets/job_probing_principle.png)

For zeroing Z only, a probe can boil down to a piece of \(conductive\) metal sheet \(of know thickness\), for zeroing X, Y and Z, it needs to be a 3-dimensional but the principle is the same. X/Y/Z probes typically have a recessed face on the bottom side, so that they can be placed pushed against a corner of the stock top surface:

![](.gitbook/assets/job_3dprobe_dimensions.png)

{% hint style="info" %}
Probes come in two types: passive and active. Passive is just what was described above, i.e. a glorified metal cube. Active probes have internal electronics to support features like an embedded test LED that lights up when contact is made, which is VERY useful for checking that the probe chain is working fine before initiating the probing cycle
{% endhint %}

The probing goes like this:

![](.gitbook/assets/job_3dprobe_principle.png)

* The probing cycle starts with the tool raised above the probe. It could be anywhere above, but there is usually a target area above which to \(coarsely\) position the tool, to facilitate the rest of the sequence
* the tool is lowered along Z, until it makes contact. When it does, the software can just read the current Z and subtract the thickness of the probe \(PZ in the sketch above\) to get Z0. This Z touch off sequence can be repeated to average out values and improve precision
* if the probing cycle is configured to also probe X and Y,  it retracts the tool and proceeds to move to the left side, past the edge of the probe, lowers the tool and then comes back towards the left side of the probe until it detects contact: it can then determine X0 by reading the current X value, add PX, and also add half the \(preconfigured\) diameter D of the tool itself. Probing Y is similar.

If the stock shape does not have a straigth corner, the probe can also sit on top of the stock completely, aligned manually against one side, and probed on that side only. Or just used for Z probing only.

One problem remains: the X0/Y0 computations depend on \(half the\) diameter of the tool, so the geometry of the tool must have been configured beforehand, and this manual operation is error prone. Also, the _actual_ precise diameter is often not quite the advertised value, so this will introduce a _slight_ error in X0/Y0, which will result in a shift between runs with different tools. One more probing trick can be useful: if the probe has hole, one can lower the tool into the hole and then probe the its sides : probe left and memorize current X value, then probe right and memory current X value : the average \(middle\) of these two values is at the X center of the hole. Repeating this operation by probing on the front/back side of the hole will locate the Y center of the hole. Since the location of the center of the hole is at a known distance from the inner corner of the probe, this gives X0 and Y0. Z-probing can then be done normally elsewhere on the top surface. The beauty of this method is that it is independent of the tool diameter ! 

![](.gitbook/assets/job_probing_hole.png)

## Running the job

TODO: Carbide motion, retract policy

## Tool change

* **safety**: if you are using a router of spindle that is externally controlled, I recommand actually cutting power \(wherever this is done in the chain\). If you are manually turning the router on and off, this is less of a risk BUT I choose to be extra cautious, and also kill router power at the source \(in my case, on the control panel that powers everything\) 
* 
## Crashing the machine

TODO: be prepared, don't overthink it 

limit switches = homing, not limit protection

 soft limits 

zeroing issues

 belt issues 

inspection after a crash





