# Introduction

Version 1.0.0 / July 2019

The intent of this guide is to help new users of the Shapeoko \(or any other hobbyist CNC, really\) learn enough about both the big picture and the underlying technical concepts, to feel at ease with the machine and how it behaves. 

This is a personal initiative, not affiliated with Carbide3D company in any way, yet positively biased \(it's such a great machine, what can I do...\) 

With so many different kinds of users of the Shapeoko, there are many ways to get started. Some people will want to be up and running and making things as quickly as possible, without the burden of learning all the nitty gritties, and that's fine. At the other end of the spectrum, some users \(including yours truly\) consider CNC and the Shapeoko as a hobby, and end-up spending more time experimenting with the machine than actually cutting useful projects, and that's fine too. Hopefully there is a little something for everyone in this guide.

As fun as it is to go ahead and cut a first project following a tutorial \(and you should\), having a good understanding of how the machine works and what underlying concepts it uses goes a long way to avoid frustration later, when things do not happen they way you expected \(and they will\).

## Safety considerations

This is not \(only\) a legal disclaimer, this is plain old common sense but needs to be said: using a CNC comes with a small number of specific risks for your safety, that are easily adressed:

* **mechanical injury**:
  * \(obviously\) keep your hands away from the moving parts during operation, cleaning up that mess on the belts/pulleys can probably wait until the end of the job.
  * Get yourself a way to do an emergency stop. A killswitch removing ALL power \(to the machine and to the router\) is easy to install, within arm's reach.
  * \(easy to forget\) turn off the machine before diving in to check/adjust/fix something. Sure, the machine is "not supposed" to move by itself, but user errors happen \(ever clicked anywhere by mistake?\), and software has bugs. Changing an endmill while the machine is still powered on is probably the only sensible exception.
  * \(easy to get tired of\) wear protection goggles anytime the machine is operating without an enclosure, no exceptions. Endmills break from time to time, and may fly out in random directions when they do. 
* **respiratory hazard**: cutting some materials \(e.g. MDF\) produces dust that can be quite toxic if you inhale it repeatedly. 
  * A good dust collection system will take care of this risk. You'll find out soon enough if you don't use one \(and/or an enclosure\), that a thin layer of dust ends up covering everything around the machine, including the inside of your lungs.
* **fire hazard**: while this risk is very remote, it does exist.
  * don't leave your machine \(completely\) unattended while it is cutting. The stock material may become loose, and rubbing a piece of metal spinning at thousands of RPM against wood is a potential way to start a fire. Google "CNC" + "fire" if you need proof.

## **Overview**

There are many ways to learn and use the Shapeoko, but the typical experience goes something like this:

* Purchase the machine \(after reading a lot of good things about it\) 
* While waiting for delivery, get familiar with CNC concepts 
  * This is the intent of the first sections \([Workflow](workflow.md), [Coordinate system](), [Anatomy of a Shapeoko](anatomy-of-a-shapeoko.md), [CAD, CAM, G-code](cad-cam-tools.md), [Cutters & collets](cutters.md), [Feeds & speeds](feeds-and-speeds-basics.md), [Toolpaths](toolpath-basics.md), [Workholding](workholding.md)\). They go in way more detail than is strictly necessary to get started, so if it feels overwhelming just skip the details.
  * The [Cutters & collets](cutters.md) section includes a few indications as to what starter set of endmills and collets could be suitable for you, depending on the projects you have in mind. 
* Receive and assemble the machine, and run the first Hello World. Oh the joy of these first few hours! I chose _not_ to have a section about machine assembly, because Carbide3D's docs are quite detailed already \(still, take them with a grain of salt, they are not 100% foolproof & unambiguous\), and there are many things that are either specific to the machine type \(standard vs XL vs XXL\) or bound to change frequently and render this information obsolete. Carbide3D's support email and community forum are the best way to sort out any specific machine assembly.
* To run your first cuts in wood or MDF, learn about [Homing, Zeroing, Cutting](first-cuts.md), then anxiously click on the "run" button for the first time. 
* Realize that:
  * CNC is messy, and now is a good time to look into [Dust collection, Enclosures](dust-collection.md)
  * The geometry of cut pieces or finish quality is not quite right, and that it may be necessary to look into [Squaring, surfacing, tramming](squaring.md), and possibly [Dimensional accuracy](x-y-z-calibration.md).
* Try cutting plastics or metal \([Usecases: cutting plastics](cutting-plastics.md) , [Usecases: cutting metal](cutting-metal.md)\)
* At some point, witness the machine misbehave, blame the hardware or the software, start [Troubleshooting](), and \(often\) end up concluding that you messed up somewhere in the workflow.
* Somewhere along the path, outgrow Carbide Create and Carbide Motion and consider moving to more advanced [CAD, CAM, G-code](cad-cam-tools.md) sender tools.
* Perform a little [Maintenance](maintenance.md) from time to time, to keep the machine in optimal working conditions.
* After a while, begin to feel an itch for [Upgrading the machine](upgrading-the-machine.md)

## **Feedback & credits**

Comments/corrections are most welcome, you can contact me on the Shapeoko community forum \([**https://community.carbide3d.com/**](https://community.carbide3d.com/)\) as **@Julien**.

Special thanks to **@snaterst** \(on the forum\) for providing the vise setup picture, and to **@Dusty.Tools** \(on Instagram\) for the T-tracks setup picture.

This is e-book released under the Creative Commons CC BY-NC-SA license \(in plain english: do whatever you want with it except a commercial use, and if you modify & redistribute, credit and keep the same license\)





