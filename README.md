# Physical Verification using skwater 130 nm Technology

The need for physical verification is imperetaive for developing a design that is manufacturable, having a high probaility of obtaining a GDSII layout that can be successfully taped out. Thanks to the development of openSource tools that has been a driving impetus for democratizing chip design and almost (if not fully) has automated the chip deisgn flow (RTL-to-GDSII).

The RTL is a register transfer level netlist of the chip that is in the making. One of the most important process is going from the RTL (from HDL languages) to a complete layout is that is the GDSII format of layout format that can be sent to the foundry for manufacturing the chip.  

## Content
  - [Day 1](#day-1)
    - [Check Tool Installations](#check-tool-installations)
    - [Creating Sky130 Device Layout in Magic](#creating-sky130-device-layout-in-magic)
    - [Creating Simple Schematic in Xschem](#creating-simple-schematic-in-xschem)
    - [Creating Symbol and Exporting Schematic in Xschem](#creating-symbol-and-exporting-schematic-in-xschem)
    - [Importing Schematic to Layout and Inverter Layout Steps](#importing-schematic-to-layout-and-inverter-layout-steps)
    - [Final DRC/LVS Checks and Post Layout Simulations](#final-drc/lvs-check-and-post-layout-simulations)
    
  - [Day 2](#day-2)
    - [GDS Read](#gds-read)
    - [Port](#ports)
    - [Abstract Views](#abstract-views)
    - [Basic Extraction](#basic-extraction)
    - [Setup for DRC](#setup-for-drc)
    - [Setup for LVS](#setup-for-lvs)
    - [Setup for XOR](#setup-for-xor)

  - [Day 3](#day-3)
    - [Lab for Width Rule and Spacing Rule](#)
    - [Lab for Width Spapcing Rule and Notch Rule](#)
    - [Lab for Via Size, Multiple Vias, Via Overlap and Autogenerate Vias](#)
    - [Lab for Minimum Area Rule and Minimum Hole Rule](#)
    - [Lab for Wells and Deep N-Well](#)
    - [Lab for Derived Layers](#)
    - [Lab for Parameterized and PDK Devices](#)
    - [Lab for Angle Error and Overlap Rule](#)
    - [Lab for Unimplemented Rules](#)
    - [Latch-up and Antenna Rules](#)
    - [Lab For Density Rules](#)
    
  - [Day 4](#day-4)
    - [](#)
    - [](#)
    - [](#)

  - [Day 5](#day-5)
    - [Simple LVS Experiment](#)
    - [LVS with Subcircuit](#)
    - [LVS with Blackboxes Subcircuit](#)
    - [LVS with SPICE Low Level Component](#)
    - [LVS with SPICE Low Level Components](#)
    - [LVS for Small Analog Block PowerOnReset 1](#)
    - [LVS for Small Analog Block PowerOnReset 2](#)
    - [LVS for Macros](#)
    - [LVS Digital PLL 1](#)
    - [LVS Digital PLL 2](#)
    - [LVS with Pproperty Errors](#)

    
### Day 1 
#### Check Tool Installations
The tools have been installed in the VSD sandbox with the PDKs installed and built. While these can be installed by cloning the github repository and then building the source from scratch. The tools and the PDKs that are used in the workshop are free and open source. They can be easily installed in Linux systems such as Ubuntu, CentOS etc.

The tools can be compiled from the source, from github. This makes sure that the tools that has been built is uptodate and has the most recent version with bugs that have been taken care of to date.

To invoke commands, use the termial with the associated binaries for the tools that have been added to the path during the installations and which can be invoked from the command line. Tools such as **magic, netgen, xschem, ngspice** are all as simple as typing the specific names on the terminal window and either getting the GUI interface or a command line interface for the tools (tcl command interface generally).

```
$ magic
```
The Figure below is the **magic** interface and the tcl console, **netgen**, **xschem** for building the schematics and ngspicie for simuation using **ngspice**. 

 <img src="images/1magic.png">
 <img src="images/2netgen.png">
 <img src="images/3xschem.png">
 <img src="images/4ngspice.png">
 
 Commands for invoking the different tools that are specific to the tasks for the verification process **magic** for layout, **xschem** for schematic, **netgen** where it is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic".
 ```
 magic -noconsole         #without console
 magic -dnull -noconsole  #to be invoked while **batch mode** and when invoked from a script
 magic -dnull -noconsole foo.tcl
 
 netgen -noconsole        #without console
 netgen -batch source foo.tcl
 
 xschem --tcl foo.tcl -q
 
 netgen -b  # batch mode
 ```
 
There is a need to setup the directory hierarchy for running the tools for a particular project that follows the same structure throughout projects.


```
mkdir xschem
mkdir mag
mkdir netgen

ln -s /usr/shapep/pdk/sky130A/libs.tech/xschem/xschemrc
ln -s /usr/shapep/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit
cd ../mag
ls -s /usr/shapep/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
cd ../netgen
ln -s /usr/shapep/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl setup.tcl
```
#### Creating Sky130 Device Layout in Magic

The Magic tools shows the technology associated linked with the **magic** instance can be run with a command **magic -d XR** for better and saturated display of the layers and text values.
 
 Can **quit** with the **quit** command on the console or the **File->Quit** optin from the GUI.
 
 <img src="images/6magic_after_setup.png"> 
 
The below figure shows a lot of examples that can be explored. The link shows as blocks on left of the windows by clicking on it and type **e** for enter, typing **ctrl+e** to get back.
 
 <img src="images/5xschem_after_setup.png">

There are various tips to work with the magic tools for drawing, selecting and placing the layers:

- **Left** and **Right click** to create a box for placing layer values
- Middle mouse button to add the layer -> **painting**
- **e** to erase after creating the boxes using the right and left clicks of the mouse
- Can be panned and zommed using the scroll 
- Layout can be created using the schematic or also can be drawn freehand.
- **Devices1** and **Devices2** can be selected from which the device parameters for NMOS and PMOS can be changes, that have been selected.
- click on **v** to view the layout if it pans out of the viewing area.
- From the paramerters menu things like **guard rings, transistors from device type can be changed, guard value** can be changed. If the window is closed can be opening using **ctrl+p**
- If you are working in the layout type **;** andn type a command without going to the command window.
- **what** command to know about the selection in the layout window
<img src="images/7painting.png">
<img src="images/8nmos.png">

#### Creating Simple Schematic in Xschem

Creating the chematic
- File **New Schematic**
- The windows contains three different libraries 
  - Working Directory
  - Skywater PDK (select for the MOSFETs)
  - Xschem generic library for voltages and other for simulations
- Select from the library **nfet_01v8.sym** need to select a four terminal device as the bulk is necessary and as in 3 terminal device it is mentioned as a parameter.
- Libray can be opened again using the **insert** key from the keyboard.
- Same thing to select the pfet. (select the fet without the bulk terminal)
- Since pins are not speciic to the technology or library so select it from the generic library.
- **M** ket can be used to move it. (place the **ipin**, **opin** and **iopin**)
- **W** to wire the components.
- **Delete** to delete the components.
- Changing the pin names by cliking and typing **q** to replace the **xxx** values to a resonably unstandable names.
- select nfet and type **q** to change the values to **L=0.18, W=4.5, nf=3**, for pfet **L=0.18, W=3, nf=3**

<img src="images/9inverter.png">

#### Creating Symbol and Exporting Schematic in Xschem

- The testbench is created to check the functional validation of the schematic that has been created by keeping it isolated with the schematic file that has been created.
- The Volatage sources can be drawn from the generic library
- click and select the voltage and type **q** and set to **1.8**
- value of the input voltage to the inverter needs to be put to **"PWL(0 0 20n 0 900n 1.8)"**
- Add codes for running the simulation
  - add from the library and select **code_shown.sym**
  - Get the device models from the library **value = ".lib /usr/shar/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt"**, where tt is the process corner.
  - Add a second code block for simulating with the given change as **value = ".control \
  tran 1n 1u \
  plot V(in) V(out) \
  .endc"**
  - From the xschem window click on the **netlist** opption on the top right and then select **simulate** to run the simulation, it takes a while to start as it loads all the library elements.
  - It runs and then creates a plot.
  - After the simulation the schematic (of which the layout needs to be created has to be selected ans then from **schematic** select **LVS netlist: Top level is a .subckt**. This ensures that the subcircucit is propeprly defined for creating the layout with pinsn in the layout.
  - Then finally click on the netlist again to generate a netlist.
<img src="images/10tb.png">
<img src="images/11plot.png">
<img src="images/12spicerun.png">

#### Importing Schematic to Layout and Inverter Layout Steps
#### Final DRC/LVS Checks and Post Layout Simulations

### Day 2 
#### GDS Read
#### Port
#### Abstract Views
#### Basic Extraction
#### Setup for DRC
#### Setup for LVS
#### Setup for XOR

