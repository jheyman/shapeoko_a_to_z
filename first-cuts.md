# Running a job

The basic workflow for running a job is covered pretty well in Carbide3D's docs/tutorials, the intent of this section is to provide background information and various tips about the different steps.

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

The reason for homing was covered in the [Coordinate system](coordinate-system.md) section: the only way the Shapeoko can tell for sure where it is, is when it contacts the three limit switches. Without homing, each time the machine is power cycled it would then be unable to go back to any specific coordinates, e.g. if the rails have been moved manually in the meantime.

You could argue that homing is useless since you are going to manually jog to the Zero point and set it to be the reference anyway. For a job that can be done in a single run / with a single tool, that could work. but the power of homing is that it will allow to return with great precision to Zero point defined last \(which happens to be stored in the non-volatile memory of the controller, by the GRBL software\). 

If you turn off the machine to change tools, this will be needed. But even if you do not turn off the machine to change tools, you may still want to home between runs: if for some reason the steppers or belts skipped a few steps during the previous run, returning to Zero without homing first _may_ bring the machine to a subtly different point, a few steps away. If you are doing a 2-sided job or anything that requires precision and uses successive passes with different tools, these few steps may matter.

## Zeroing \(manually\)

After jogging to the vertical of the intended zero point on the stock surface, lower Z gradually then use fine steps to touch off on e.g. a piece of paper placed between the tool and the stock, stopping as soon as the paper cannot be moved freely. Yes, it does mean that you will zero a tiny bit above the real stock surface, but the average thickness of a piece of paper is around 0.004" / 0.1mm, and chances are you are already compressing the paper since it cannot move anymore, so you will actually be very, very close to stock surface.

*  Double-check you Z jogging step before touching off. It is very easy to hit on the "down" arrow by mistake, and if the step is still say 0.1", this might bury your tool into the stock, or more likely break it if it is a small endmill
* Touching off with a very pointy V-bit can be tricky, it's easy to go too far down without noticing  use even finer jog steps and/or extra caution.
* TODO finish this part

## Zeroing \(with a Probe\)

xxx

## Setting RPM 

TODO: router : RPM Iin g-code but ignored / set manually/ control with tachymeter

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





