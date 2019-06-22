# Maintenance/troubleshooting

## Hitting the limits

On one end of each axis, the homing switches will interrupt the toolpath if triggered, this case is the easy one because GRBL knows something is wrong and stops everything.

On the other end of each axis, since there is no way for the machine to know if it went too far, there are two cases:

* either the "soft limits" in GRBL have been activated and configured to the correct value for you machine, and the job will stop
* or the soft limits are turned off, and you can get a mechanical crash

TODO pro/cons of soft limits

## Crashing the mashine

This will happen sooner or later, and it will very likely be a user error. 

Checklist: inspect belt, wheels, look for play in the gantry

## Disconnects



## Resurfacing the wasteboard



## Replacing endmills

xxx

TODO: especially recommanded feeds & speeds & doc do not seem to work

## Cleaning belts & wheels

xxx

## Tightening belts & wheels 

xxx

## Pulleys

![](.gitbook/assets/pulley_setscrew.png)

