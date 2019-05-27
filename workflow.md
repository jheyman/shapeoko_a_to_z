# Workflow

First things first, it is important to understand the workflow of a typical CNC job: 

![](.gitbook/assets/workflow800.png)

Everything starts in a **CAD** \(Computer-Aided Design\) tool: this is where you will create the 2D or 3D objects to be machined. CAD software packages are usually able to import 2D and 3D features from a variety of file formats, and the most common/useful ones for CNC are "vector" formats. 

_**Carbide Create**_, the CAD program provided by Carbide3D for the Shapeoko, can import SVG or DXF vector files, as well as Bitmaps \(but only to be used as background references for manual tracing\) 

Once the object is designed, the **CAM** \(Computer-Aider Manufacturing\) tool is used to create **toolpaths** to cut the object out of a block of material. More on this later. Once all required toolpaths are created, the very last step in the CAM program is to generate a **G-code** file that will contain instructions for the machine to execute these toolpaths.

G-code **format** is a standard \(originally ISO 6983-1 back in the 80's\) so one would expect that a G-code file can be run on any CNC. Well almost, but not quite. There is a wild range of CNCs, that support various subsets of the G-code instructions, as well as implement their own custom instructions.

Since CAM programs are usually not bound to any specific CNC, they make use of a specific **post-processor** to generate the correct G-code for a selected target machine.

Note: in _**Carbide Create**_, the G-code post-processor is executed behind the scenes, and it knows what model you have since there is a dedicated "Machine" parameter in the job setup.

Finally the instructions from the generated G-code file must be executed by the machine to produce the required movements of the router to cut through the material. This requires a **sender**, that goes through the G-code file line by line, and sends the instructions to the machine, or more precisely to the machine's **controller**, via a communication link \(USB on the Shapeoko\). **Carbide Motion** is Carbide3D's G-code sender.

The controller executes a piece of software that interprets incoming instructions, and translates them into specific movements of the X, Y, and Z axis. On the Shapeoko, this software is "**GRBL**", \(pronounced "Gerbil"\), an open source motion control software \(see [https://github.com/gnea/grbl](https://github.com/gnea/grbl)\)

Note: the G-code also contains instructions to control the **rotation speed** \(RPM\) of the trim router, as defined in the CAM program. On a stock Shapeoko, 





  





CAM tool

