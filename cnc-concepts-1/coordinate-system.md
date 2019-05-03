# Coordinate system

The coordinates system is one of those things that can be a little confusing at first. The axis definitions themselves are straightforward:

* **X** is the left-right axis, with values increasing from left to right
* **Y** is the front-back axis, with values increasing from front to back
* **Z** is what you would expect, vertical axis pointing up, so the "altitude" if you will.

The next question is, where is the origin ? On a CNC like the Shapeoko, there is no feedback telling the machine where it is positioned in space, so the only thing it can do is control X/Y/Z movements **relative** to a given starting point, i.e. "_go 1" to the right_"

The **ZERO** point \(X0,Y0,Z0\) is the point in space against which all movements for a job will be referenced.

![](../.gitbook/assets/coordinate_system800.png)

This point is usually referenced somewhere on the stock material \(e.g. a corner, or the center of the top face\), but it can be set anywhere in the workspace. The G-code for a given job will use this reference, and perform movements **relative** to this local origin. So, to successfully cut a piece, manually setting such a Zero position is enough.

However, the machine also has a **Home** position, which is a specific predefined point in space that the machine can go to when it needs to reset its location: the Home position corresponds to somewhere where the machine does get a feedback that it has reached the position, and on the Shapeoko that's the back right corner, where **limits switches** on the X and Y axis are triggered. 

**Homing** consists in telling the machine to move in the direction of positive X and positive Y and positive Z, until it detects that each associated limit switch has triggered, and stop movement on the corresponding axis then. Once all three limits switches have been triggered, the machine is guaranteed to be in a known position \(mechanically\), i.e. Home. 

If a G-code file is executed from an arbitrary Zero point, why does it matter where Home is ? The trick is that the **coordinates of the Zero point** itself, are defined with respect to the Home position, and happen to be stored in the permanent memory of the machine. So when the machine is in an arbitrary position and is turned off, the next time it will be turned on, Homing allows to go back to this known **absolute** position, and from there return to the previous Zero position.

