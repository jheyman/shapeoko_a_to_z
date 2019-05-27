# G-code primer

Ok, so the GRBL software in the Shapeoko controller eats G-code, and it is the job of the G-code sender \(e.g. Carbide Motion\) to feed it. Since the purpose of CAM tools is to generate G-code files, one can perfectly use the Shapeoko without any understanding of the G-code syntax, and without ever opening a generated G-code file. However, having at least a superficial understanding of what the most common G-code instructions do goes a long way when in need of troubleshooting why the machine behaves the way it does.

G-code is a \(somewhat\) standard language to define instructions to be fed to a multi-axis machine, CNC and 3D printers being prime examples.

A G-code file is usually a **plain text** file \(that can be opened with any text editor\), containing a sequence of G-code "blocks", i.e. lines of instructions.

Each block \(line\) can contain several G-code instructions, but it often contains a single command and its parameters, for better readability.

These instructions are being sent \(by e.g. Carbide Motion\) to the machine, that executes them **in order**. 

{% hint style="info" %}
G-code standard does define loop/jump instructions \(GOTO\), but they are not supported by GRBL anyway, so on a Shapeoko the G-code is guaranteed to execute from the top to the bottom of the file sequentially.
{% endhint %}

Here's the beginning of a random G-code example generated using Carbide Create \(other CAM tool produce slightly different variations of the code\) :

```text
%
(TOOL/MILL,0.1,0.05,0.000,0)
(FILENAME: )
()
G21
G90
G0X0.000Y0.000Z10.000
(TOOL/MILL,1.5875,0,1.0000,0.0)
M6 T112
M3 S10000
G0X0.000Y150.000
G0Z10.000
G1Z-0.250F300.0
G1X150.000F1200.0
Y0.000
X0.000
Y150.000

[...many other G-code instructions...]

G0Z10.000
M5
M30
(END)
```

Let's break this down:

* the **%** line just means "start of program". Well it actually means "Tape start", from the days where CNC used tape readers. Anyway, this sign is optional, you will find some CAM tools add it, others don't.
*  Everything between parenthesis is a **comment**, and is ignored by GRBL. This is just there for readability of the G-code file.
* **G21** is the command to set **units** to mm \(G20 being the command to set units to inches, instead\)
* **G90** is the command to go to **absolute positioning** mode, i.e. all subsequent move commands will interpret X/Y/Z values as coordinates relative to the origin of the machine.
* **G0** is the **Rapid Move** command, it takes X and/or Y and/or Z and/or Feedrate values as parameters, and is intended to reposition the tool to a different location while not cutting anything on the way.
* **M6 T112** corresponds to a **Tool Change** command, requestion tool number 112 in this example.
* **M3 S10000** instructs the controller to turn the **Spingle on**, at 10.000RPM in this example.
* **G1** is the **Linear move** command, similar to G0 but intended to be used for actual cutting moves, that typically happen slower than the G0 rapid moves. The feedrate for these moves can be specified on the line, that's the **Fxxx**" part an the xxx is the feedrate value in units \(inch or mm\) per minute.
* Standalone coordinates like **Y0.000, X0.000, Y150.000** are in fact implicit/short versions of G1 move commands: as long as the movement mode does not need to be changed, the latest G commands applies and in this example the "G1" can be omitted from these lines.
* **M5** is used to turn the **Spindle off**.
* **M30** means "End of program" in GRBL.

{% hint style="info" %}
* On a stock Shapeoko with a trim router, there is no automatic control of the router activation nor RPM, so the Spindle commands have no effect
* The Shapeoko does not have an automatic tool changer, but Carbide Motion uses the M6 tool change command
* Spaces inside a line are ignored
{% endhint %}

Beyond this basic example, a few more common commands are:

* **G2/G3** commands to do **Arc moves** 
* **G17** explicitly selects the XY plane as the plane in which these arc moves are done
* **G94** explicitly sets the feedrate values to be "units \(inch or mm\) **per minute**"
* **M8/M9** is Coolant on/off. Not applicable/ignored on the Shapeoko, but may be present depending on the post-processor used
* **G54** is the command to select "coordinate system \#1", a.k.a. the coordinate system that is based on the Zero point you set. This command is optional since G54 is the default at GRBL startup anyway.

There are many, many more G-code commands, but basically the commands above will cover 99% of the need on a Shapeoko.

















