# Introduction

The intent of this guide is to help users of the Shapeoko \(or any other hobbyist CNC, really\) learn enough about both the big picture and the underlying technical concepts, to feel at ease with the machine and how it behaves. 

This is a personal initiative, not affiliated with Carbide3D company in any way, yet positively biased \(it's such a great machine, what can I do...\) 

With so many different types of users of the Shapeoko, it is quite clear that there are many ways to learn. Some people will want to be up and running and making things as quickly as possible, without the burden of learning all the nitty gritties, and that's fine, the Shapeoko was intended to fulfill this usecase. At the other end of the spectrum, some users consider CNC and the Shapeoko as a hobby, and end-up spending more time improving the machine than actually cutting, and that's fine too. Hopefully there is a little something for everyone in this guide, but it leans towards the latter category of people.

As fun as it is to go ahead and cut a first project following a tutorial \(and you should\), having a good understanding of how the machine works and what underlying concepts it uses goes a long way to avoid frustration later, when things do not happen they way you expected \(and they will\).

In other words, you do not _need_ to know about many of the details presented in the following sections, but chances are it could help you down the line.

Comments/corrections are most welcome, you can contact me on the Shapeoko forum \([**https://community.carbide3d.com/**](https://community.carbide3d.com/)\) as @Julien.

## Safety considerations

This is not \(only\) a legal disclaimer, this is plain old common sense but needs to be said: using a CNC comes with a small number of specific risks for your safety, that are easily adressed:

* **mechanical injury**:
  * \(obviously\) keep your hands away from the moving parts during operation, cleaning up that mess on the belts/pulleys can probably wait until the end of the job.
  * Get yourself a way to do an emergency stop. A killswitch removing ALL power \(to the machine and to the router\) is easy to install, within arm's reach.
  * \(easy to forget\) turn off the machine before diving in to check/adjust/fix something. Yes the machine is "not supposed" to move by itself, but user errors happen \(ever clicked anywhere by mistake?\), and software has bugs. Changing en endmill while the machine is still powered on is probably the only sensible exception.
  * \(easy to get tired of\) wear protection goggles anytime the machine is operating without an enclosure, no exceptions. Endmills break from time to time, and may fly out in random directions when they do. 
* **respiratory hazard**: cutting some materials \(e.g. MDF\) produces dust that can be quite toxic if you inhale it, repeatedly. 
  * A good [Dust collection](dust-collection.md) system will take care of this risk. You'll find out soon enough if you don't use one \(and/or an [Enclosure]()\), that a thin layer of dust ends up covering everything around the machine, including the inside of your lungs.
* **fire hazard**: while this risk is very remote, it does exist.
  * don't leave your machine \(completely\) unattended while it is cutting. The stock material may become loose, and rubbing a piece of metal spinning at thousands of RPM against wood is a potential way to start a fire. Google "CNC" + "fire" if you need proof.

\*\*\*\*







