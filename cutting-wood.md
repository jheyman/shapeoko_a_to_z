# Usecases: cutting wood

## General tips

Grain direction versus toolpath

Finding the sweet spot for feeds and speed

TODO: starting point + experimental approach + chatter signs

## Bamboo tool holder

Ok, technically bamboo is not wood, but close enough. In this example, I wanted to make a holder for my endmills from a cheap \(Ikea\) bamboo cutting board. I used the tape&glue method to secure it to the wasteboard

{% hint style="info" %}
I added extra pieces of about the same thickness on the sides and front, this allows the dust shoe to not lose suction when it moves past the edge of the stock during the toolpath.
{% endhint %}

![](.gitbook/assets/bamboo_holder_stock.png)

The design &lt;TODO&gt;

![](.gitbook/assets/fs_usecases_toolholder_design.png)

Since I needed to cut a large number of deep pockets with tight corners, I first created a roughing toolpath using a large \(6mm\) endmill, 

Roughing: 6mm 2flutes, 12.000RPM, FR 2000mm/min, 3D adaptive clearing, optimal load 0.8mm, plunge rate 400mm/min, target chip per tooth 0.07mm, corrected for chip thinning = 0.083mm, 0.5mm radial stock to leave, 14.2mm DOC \(full depth\)

![](.gitbook/assets/fs_usecases_toolholder_roughing_toolpath.png)

xxx

![](.gitbook/assets/bamboo_holder_roughing.png)

Finishing: 3.175mm 2flutes downcut, 12.000RPM, FR 840mm/min, 3D adaptive clearing with Rest machning, optimal load 0.55mm, plunge rate 400mm/min, target chip per tooth 0.035mm, corrected for chip thinning = 0.0458mm, 14.2mm DOC \(full depth\)

![](.gitbook/assets/fs_usecases_toolholder_finishing_toolpath.png)

vvvv

![](.gitbook/assets/bamboo_holder_finished.png)





