# Workholding

Workholding is confusing at first because there are many ways to do it, and everyone seems to have their own tricks. The goals are:

* \#1: obviously, that the workpiece will not come off during the cut, which would both ruin the piece AND could be dangerous \(flying objects, machine damage, fire...\) 
* \#2: to provide rigidity of the machine+stock setup. If the workpiece or workholding bends/shifts slightly under cutting forces, cut quality will be poor, and dimensions will be off. 
* \#3: to maximise clearance/access to the faces of the stock to be machined 
* \#4: that it is easy/convenient enough to install and remove the workpiece.

## Wasteboard

The majority of projects require to cut all the way through the stock material, if only to cut around the outer profile of the workpiece. Having a wasteboard placed under the stock material is very common for at least the following reasons:

* Even on a properly calibrated machine, it is not easy to cut _exactly_ down to stock bottom.
* It is often not desirable anyway: the cut quality on the bottom of the piece can be poor if not overcutting \(e.g. small variations in depth/flatness would leave material\)
* Mistakes in the CAM design/G-code, or mechanical issues, might \(will!\) cause the endmill to cut deeper than expected, it would be too bad to damage the machine bed.
* The stock must lay on a surface that is flat and square to the machine's Z axis, and the simplest way to achieve this is to use the machine itself to surface its own base \(see [Squaring, surfacing, tramming](squaring.md)\), so it should be a replaceable part.

It is _possible_ to use the Shapeoko's MDF bed as a wasterboard, but who wants to be replacing the bed of their machine \(considering it would means disassembling it, reassembling with a fresh bed, and then squaring everything again\). It is much more convenient to use a supplementary wasteboard

&lt;TODO sketch with stock + wasteboard + bed + show Z travel&gt;

The \(only?\) downside of using a supplementary wasteboard is that it reduces the maximum Z travel \(i.e. the maximum possible height for a workpiece\)

{% hint style="info" %}
The exception is using a vice to hold the stock, in which case a wasteboard is irrelevant.
{% endhint %}

Wasteboard material is very often MDF \(cheap & easy to procure\), HDPE is another option \(much more expensive, but does not wear off / tear out as easily, and is immune to humidity variations\)

The wasteboard needs to be held onto the machine's bed, in such a way that it can easily be removed. It is usually bolted on. 

## Threaded table & clamps

The most common way to hold the stock onto the wasteboard is to use clamps, with threaded holes in the wasteboard:

&lt;TODO sketch clamps, from gitblog&gt;

The back of the clamps should be higher than the front, so that the clamping pressure actually goes on the stock itself.

&lt;TODO sketch good/bad&gt;

The main drawback of this method is that the stock area where the clamps are is lost and one should carefully design toolpaths such that there is no risk of collision between the tool \(and the dust shoe around it\) and the clamps. The usual mitigations are:

* use a stock larger than strictly necessary for the workpiece, and machine only the center area of the stock. Easy, but not very efficient in terms of how much material is needed.
* use low-profile clamps: this addresses the issue of collision with the dust shoe itself, as the clamps can slide under the bristles of the dust shoe. The risk of collision with the tool itself is still to be managed. 

Since clamps only hold the stock by its edges, contour cuts leave a middle piece that gets separated from the main stock after the last pass, and this needs to be managed too: at best it can damage the piece \(it may move freely right and bounce right into the endmill\), and at worse it can be dangerous if the cutout piece is pushed and flies away.

One solution is to use **tabs** in the design, most CAM tools support this:

&lt;todo screenshot&gt;

The tabs will hold the piece during the last passes of the cut, but they will have to be removed manually afterwards, which can turn out to be 

In some materials \(e.g. soft plastics like HDPE\), an alternative option is to leave a thin "onion skin" at the bottom of the profile cut, thick enough to keep the piece from moving, but thin enough to be easily cut manually with e.g. a X-acto blade. The bottoms edges of the cut still need to be cleaned-up manually, but in soft plastics this can be very easy with a deburring tool

&lt;todo deburring tool picture&gt;

Anyway, to use clamps one must have threaded holes in the wasteboard \(or bed\). A popular option is to use the Shapeoko to drill a sea of holes in its own wasteboard, and then place threaded inserts in them.

&lt;todo pictures&gt;

## Tape & glue

Another very popular workholding method is surprisingly simple, and surprisingly efficient. It uses painter's masking tape, and CA glue:

&lt;todo sketch en coupe&gt;

&lt;TODO photos du process&gt;

* apply masking tape on the back side of the stock, using several stripes if needed to have a large area covered
* apply masking tape on the top of the wastebard where the stock will be, to match the tape under the stock
* apply a zig-zag of CA glue on the tape under the stock
* position the stock on the wasterboard and push down firmly, and across the whole surface, for a few seconds. So use glue accelerator, but this is typically not necessary.
* at this point, the stock should be held tightly : apply lateral force on it and make sure it does not budge.

Enjoy cutting the stock without any obstruction of the top and sides. When the job is finished, use a flat tool \(e.g. scraper\), slide it under one corner/edge of the stock, and apply an upward force to pry the stock free

&lt;photo&gt;

It may take a few steps, working on each corner/edge. Finally, clean-up the \(now torn\) tape from the piece and the wasteboard.

The main drawback of this method is that for all cuts that go through the full depth of the stock, during the last passes the tool will cut into the tape and glue sandwich, which is bound to leave gummy residue on the tool.

&lt;photo&gt;

This is very easily removed with a little bit of alcohol/acetone

{% hint style="info" %}
This may turn out to be a problem for jobs that do multiple profile cut: if the first profile cut leaves tape & glue residue on the tip of the endmill, it could impact the quality of the subsequent cuts. There is no real solution for this, the only partial mitigation is to cut exactly down to stock bottom to avoid cutting into the tape, but that brings back other issues if the stock bottom is not perfectly flat or the machine not perfectly trammed.
{% endhint %}



## Vice

xxx

