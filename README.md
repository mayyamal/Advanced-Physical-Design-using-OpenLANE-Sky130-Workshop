# Advanced-Physical-Design-using-OpenLANE-Sky130-Workshop

We are using picorv32a dsign (SoC).

The goal of openLANE is to produce a clean GDSII layout with no human intervention. OpenLANE already includes large number of design examples.
OpenLANE is based on several open-source projects: openROAD, Yosys, ABC, KLayout, Fault, QFlow, Magic

## Day 1 Inception of open-source EDA, openLANE and SkyWater 130

### Docker commands
- From `‌/Desktop/work/tools/openlane_working_dir/openlane` we should start the docker, and execute the following commands:
- ![image](https://user-images.githubusercontent.com/57360760/182897041-3674f729-4e5d-4a5b-8b61-57ae81fab4ce.png)
- 



Synthesis (using yosys) converts the picorv32a RTL to a gate-level netlist using the skywater??? standard cell library (SCL)


## Day 2 Good floorplan vs bad floorplan and introduction to library cells

As described in Day1 there are several floorplans: chip floor-planning, macro floor-planning, power planning


## Day 3 Design library cell using Magic Layout and ngspice characterization

As described in Day1 standard cells have a reular layout.


## Day 4 Pre-layout timing analysis and importance of good clock tree


## Day 5 Final steps for RTL2GDS using tritonRoute and openSTA

Timing verification: Static Timing Analysis (STA) ensures that all timing constraints are met, and that the circuit runs at the designated clock frequency.
