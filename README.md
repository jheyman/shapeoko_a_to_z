# Introduction

July 2019

The intent of this guide is to help new users of the Shapeoko learn enough about both the big picture and the underlying technical details, to feel at ease with the machine, the workflow, and the CNC lingo in general.

This is a personal initiative \(call it a fan project\), not affiliated with Carbide3D company in any way, yet positively biased \(it's such a great machine, what can I do...\) 

With so many different kinds of users of the Shapeoko, there are many ways to get started. Most people want to be up and running and making things as quickly as possible, without the burden of learning all the nitty gritties, and that's fine. At the other end of the spectrum, some users \(including yours truly\) enjoy CNC and using the Shapeoko as a hobby, and end-up spending more time experimenting with the machine than actually cutting useful projects, and that's fine too. Hopefully there is a little something for everyone in this guide.

As fun as it is to go ahead and cut a first project following a tutorial \(and you should\), having a good understanding of how the machine works and what underlying CNC concepts are involved goes a long way to avoid frustration later, when things do not work out they way you expected \(and sooner or later they will\).

## Safety considerations

This is not \(only\) a legal disclaimer, this is plain old common sense but needs to be said: using a CNC comes with a small number of specific risks for your safety, that are easily adressed:

* **mechanical injury**:
  * Keep your hands away from the moving parts during operation, cleaning up the mess on the belts/pulleys can probably wait until the end of the job.
  * Get yourself a way to do an emergency stop within arm's reach. A killswitch removing ALL power \(to the machine _and_ to the router\) is easy to install.
  * Turn off the machine before diving in to check/adjust/fix something. Sure, the machine is "not supposed" to move by itself, but user errors happen \(ever clicked anywhere by mistake?\), and software has bugs. Changing an endmill while the machine is powered is probably the only sensible exception.
  * Wear protection goggles anytime the machine is operating without an enclosure, no exceptions. Endmills break from time to time, and shrapnel may fly out in random directions when they do. 
* **respiratory hazard**: cutting some materials \(e.g. MDF or exotic wood\) produces dust that can be quite toxic if you inhale it repeatedly. 
  * A good dust collection system will take care of this risk. You'll find out soon enough if you don't use one \(and/or an enclosure\), that a thin layer of dust ends up covering everything around the machine, including the inside of your lungs.
* **fire hazard**: while this risk is very remote, it does exist.
  * don't leave your machine \(completely\) unattended while it is cutting. The stock material may become loose, and rubbing a piece of metal spinning at thousands of RPM against wood is a potential way to start a fire. Google "CNC" + "fire" if you need proof.

## **Overview**

There are many ways to learn and use the Shapeoko, but the typical experience for a new user can be something like this:

* Order the machine \(after reading a lot of good things about it\) 
* While waiting for delivery, get familiar with CNC concepts: 
  * This is the intent of the first sections \([CNC workflow](workflow.md), [Anatomy of a Shapeoko](anatomy-of-a-shapeoko.md), [CAD, CAM, and G-code](cad-cam-tools.md), [Cutters & collets](cutters.md), [Feeds & speeds](feeds-and-speeds-basics.md), [Toolpaths](toolpath-basics.md), [Workholding](workholding.md)\). They go in WAY more detail than is strictly necessary to get started, so if it feels overwhelming just skip the details and get cutting.
  * The [Cutters & collets](cutters.md) section includes a few indications as to what starter set of endmills and collets could be suitable for you to purchase, depending on the projects you have in mind. 
* Receive and assemble the machine, and run the Hello World example with a sharpie. Oh the joy of these first few hours! I chose _not_ to have a section about machine assembly, because Carbide3D's docs are quite detailed already, and there are many things that are either specific to the machine type \(standard vs XL vs XXL\) or bound to change frequently and that would render this information obsolete very quickly. Carbide3D's email support and the community forum are the best way to sort out any specific machine assembly issue.
* Run your first cuts in wood or MDF \(this is covered in [Running a job](first-cuts.md) section\). 
* Realize that:
  * CNC is messy, and now is a good time to look into dust collection and possibly an enclosure, this is covered in the [Shapeoko setup](dust-collection.md) section.
  * The geometry of cut pieces or finish quality is not quite right, and that it may be necessary to look into [Squaring, surfacing, tramming](squaring.md) the machine, and possibly [Dimensional accuracy](x-y-z-calibration.md).
* Get confused about required feeds and speeds for that particular case you want to use but for which no recommendation exists: hopefully re-reading the [Feeds & speeds](feeds-and-speeds-basics.md) section should help.
* Try cutting plastics or metal \(see [Usecases: cutting plastics](cutting-plastics.md) , [Usecases: cutting metal](cutting-metal.md)\)
* At some point, witness the machine misbehave, blame the hardware or the software, start investigating, and \(often\) end up concluding that you messed up somewhere in the workflow. The [Troubleshooting & maintenance](maintenance.md) section has a few pointers, after that the user community and Carbide3D's support are your best bet.
* Somewhere along this path, outgrow Carbide Create and Carbide Motion and consider moving to more advanced [CAD, CAM, G-code](cad-cam-tools.md) tools.
* After a while, begin to feel an itch for [Upgrading the machine](upgrading-the-machine.md)

## **Feedback & credits**

Comments/corrections/contributions are most welcome, you can contact me on the Shapeoko community forum \([**https://community.carbide3d.com/**](https://community.carbide3d.com/)\) as **@Julien**.

Shoutout to the whole forum community, the collective knowledge and experience accumulated over there and on the Shapeoko wiki is incredible. I tried to bring out _some_ of that know-how in this e-book, but it is also tainted by my own limited experience and habits, so any potential incorrect information/guidance is my bad.

Special thanks to **@snaterst** \(on the forum\) for providing the vise setup picture, and to **@Dusty.Tools** \(find him on Instagram\) for the T-tracks setup picture.

This is e-book released under the Creative Commons CC BY-NC-SA license \(in plain English: do whatever you want with it except using it for commercial purposes, and if you modify & redistribute, give credit and keep the same license\)

![](.gitbook/assets/cc-by-nc-sa.png)





