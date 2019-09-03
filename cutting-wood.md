# Usecases: cutting wood

This usecase is intended to illustrate how the information from the [Feeds & speeds](feeds-and-speeds-basics.md) section can be used to determine workable cutting parameters, for a project that uses wood as the stock material. Wood is quite forgiving, in the sense that the range of acceptable feeds and speeds is much larger than with e.g. plastics or metal. So this is by no means a recommendation about specific values to use for wood, but rather an example of how to make informed decisions about the feeds and speeds and toolpaths, which could be useful once you start cutting more difficult materials \(plastics & metal\) where feeds and speeds need to be just right.

## Bamboo tool holder

Ok, technically bamboo is not wood, but close enough. In this example, I wanted to make a holder for my endmills from a cheap \(Ikea\) 11"x18" bamboo cutting board. I used the tape & glue method to secure it to the wasteboard.

{% hint style="info" %}
I added extra pieces of similar thickness on the sides and front, this allows the dust shoe to not lose suction when it moves past the edge of the stock during the toolpath.
{% endhint %}

![](.gitbook/assets/bamboo_holder_stock.png)

The design is very straightforward but has a lot of deep pockets, with tight corners that a 1/4" endmill could not cut. And cutting of all this with a 1/8" endmill would take forever:

![](.gitbook/assets/fs_usecases_toolholder_design.png)

So I first created a roughing toolpath using a large \(6mm\) endmill, to do most of the clearing. 

* an **upcut** endmill is used, to have good chip evacuation and since there will be a finishing pass afterwards anyway to clean the rough top edges of the pockets.
* Using the **target chipload guideline** \(see [Feeds & speeds](feeds-and-speeds-basics.md#shapeoko-chiploads-guideline)\), and considering bamboo is somewhat easier to cut than "hard wood", I picked a target chipload value of 0.002"/ 0.055mm in the high-end of the range.
* While regular pocketing toolpaths would have worked fine, I chose to use an **adaptive clearing** toolpath, and with a stepover/radial width of cut/optimal load of 0.0315" / 0.8mm \(13% of endmill diameter, a very conservative value for wood\)
* After taking **chip thinning** into account for a 13% stepover, the corrected target chipload is 0.0327" / 0.083mm

$$
Chipload(adjusted) = \frac{6}{2 * \sqrt{(6 * 0.8) - (0.8)^2}}* 0.055 = 0.081mm = 0.00319"
$$

* Since my endmill has 2 flutes, and selecting 12.000 RPM as my speed, the required **feedrate** to reach that chipload value is 0.00319 \* 2 \* 12000 = 76.6ipm = 1945 mm/min
* Since the stepover is very low and the adaptive toolpath enabled it, I went for cutting the **full pocket depth** \(0.5" = 12.7mm = 200% endmill diameter\) in one pass.
* I chose 400 mm/min for **plunge rate**, which is very conservative for wood especially considering I also used helical ramping into the material.
* I left 0.5mm radial **stock to leave**, that the finishing pass would take care of.

A preview of the toolpath gave me this:

![](.gitbook/assets/fs_usecases_toolholder_roughing_toolpath.png)

and the cut proceeded uneventfully, taking about 40 minutes. Notice the fuzzies on the pocket edges though, due to the use of the upcut endmill:

![](.gitbook/assets/bamboo_holder_roughing.png)

I then created a finishing pass that would do two things: take care of cutting the corners to their final radius using a smaller \(1/8"\) endmill, and remove the 0.5mm remaining stock from the pocket walls.

* I used a **downcut** endmill, to get clean top edges. The poor chip evacuation is not an issue here since there is very little material to remove and it happens inside a large pocket with lots of space around the endmill.
* I picked a radial width of cut of 0.02" / 0.55mm, i.e. just a bit more than the stock to leave from the previous pass, so that it will only take one pass on the walls \(and just a few passes in the corners\). That is only 17% of the endmill diameter anyway.
* Then to both cut the corners and remove the stock left from the previous pass, I selected the same geometry but used the **rest machining** option: the CAM tool is aware of the material already cut during the first pass, and will generate a toolpath that cuts the rest.
* The target chipload from the guideline for an 1/8" endmill in hard wood is up to 0.001"/0.025mm, and since bamboo is somewhat softer and that I would only be cutting a thin layer of material, I aimed at a value a little higher, 0.0014" / 0.035mm.
* Adjusted for chip thinning with 0.02"/0.55mm stepover, that's a 0.0019"/0.046mm chipload/
* Considering the 2 flutes on this endmill and 12.000RPM, that requires a feedrate of 43ipm = 1100mm/min
* Still using a full pocket depth cut.
* Still using a plunge rate of 400mm/min.

Thanks to the magic of rest machining, this finishing pass is quick \(7 minutes\):

![](.gitbook/assets/fs_usecases_toolholder_finishing_toolpath.png)

Which gave me this result, right out of the machine, with perfect edges and no need for any clean-up/sanding:

![](.gitbook/assets/bamboo_holder_finished.png)





