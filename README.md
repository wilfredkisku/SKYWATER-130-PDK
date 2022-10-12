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
 
#### Creating Sky130 Device Layout in Magic
#### Creating Simple Schematic in Xschem
#### Creating Symbol and Exporting Schematic in Xschem
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

