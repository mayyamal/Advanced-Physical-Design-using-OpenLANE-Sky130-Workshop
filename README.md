# Advanced-Physical-Design-using-OpenLANE-Sky130-Workshop



The goal of openLANE is to produce a clean GDSII layout with no human intervention. OpenLANE already includes large number of design examples.
OpenLANE is based on several open-source projects: openROAD, Yosys, ABC, KLayout, Fault, QFlow, Magic

## Day 1 Inception of open-source EDA, openLANE and SkyWater 130

- From `‌/Desktop/work/tools/openlane_working_dir/openlane` we should start docker and execute the following commands:
 ![image](https://user-images.githubusercontent.com/57360760/182897041-3674f729-4e5d-4a5b-8b61-57ae81fab4ce.png)
- In this workshop we are using the **picorv32a** design: 
 ![image](https://user-images.githubusercontent.com/57360760/182905986-db82a812-0e88-4e9b-a9d7-da47cebe2a83.png)
 
- In `config.tcl` as well as in `sky130A_sky130_fd_sc_hd_config.tcl` I changed the `CLOCK_PERIOD` as show in the lab videos:
 - `config.tcl`: 
  ‌![image](https://user-images.githubusercontent.com/57360760/182913991-b851bdac-5882-455a-867b-a72f9159caf7.png)

 - `sky130A_sky130_fd_sc_hd_config.tcl`:
 ![image](https://user-images.githubusercontent.com/57360760/182912349-15911f31-7eda-46c0-9a67-5b3eeebd2456.png)
 
- Afterwards we prep the design:
![image](https://user-images.githubusercontent.com/57360760/182914183-6561f0ab-b0c5-45b5-89f4-2cd99b129ac4.png)
- In `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` a `runs` folder is created:
![image](https://user-images.githubusercontent.com/57360760/182914774-162b17c0-71da-485f-9cc7-e1029d627409.png)


 





Synthesis (using yosys) converts the picorv32a RTL to a gate-level netlist using the skywater??? standard cell library (SCL)


## Day 2 Good floorplan vs bad floorplan and introduction to library cells

As described in Day1 there are several floorplans: chip floor-planning, macro floor-planning, power planning


## Day 3 Design library cell using Magic Layout and ngspice characterization

As described in Day1 standard cells have a reular layout.


## Day 4 Pre-layout timing analysis and importance of good clock tree


## Day 5 Final steps for RTL2GDS using tritonRoute and openSTA

Timing verification: Static Timing Analysis (STA) ensures that all timing constraints are met, and that the circuit runs at the designated clock frequency.
