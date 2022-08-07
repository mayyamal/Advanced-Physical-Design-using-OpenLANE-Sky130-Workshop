# Advanced-Physical-Design-using-OpenLANE-Sky130-Workshop



The goal of openLANE is to produce a clean GDSII layout with no human intervention. OpenLANE already includes large number of design examples.
OpenLANE is based on several open-source projects: openROAD, Yosys, ABC, KLayout, Fault, QFlow, Magic.

Fully automated open-source end-to-end ASIC design flow

## Day 1 Inception of open-source EDA, openLANE and SkyWater 130

- From `‌/Desktop/work/tools/openlane_working_dir/openlane` we should start docker and execute the following commands:
 ![image](https://user-images.githubusercontent.com/57360760/182897041-3674f729-4e5d-4a5b-8b61-57ae81fab4ce.png)
 
- In this workshop we are using the **picorv32a** design: 
 ![image](https://user-images.githubusercontent.com/57360760/182905986-db82a812-0e88-4e9b-a9d7-da47cebe2a83.png)
 
- In `config.tcl` as well as in `sky130A_sky130_fd_sc_hd_config.tcl` I changed the `CLOCK_PERIOD` as show in the lab videos:
 - `config.tcl`. The variables used in this files are described in Day 2 and can be found in the `/Desktop/work/tools/openlane_working_dir/openlane/configuration/README.md` file.
  ![image](https://user-images.githubusercontent.com/57360760/183244530-350722fd-ff90-4817-ad81-2b594a7a886e.png)
 

 - `sky130A_sky130_fd_sc_hd_config.tcl`:<br/>
  ![image](https://user-images.githubusercontent.com/57360760/183244638-9ee4e49d-33f4-44aa-b1bf-08985b3c850e.png)

 
- Afterwards we prep the design:
 ![image](https://user-images.githubusercontent.com/57360760/182964268-909ea4f1-47a2-4656-8771-a5168023a7eb.png)


- In `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` a `runs` folder is created:
 ![image](https://user-images.githubusercontent.com/57360760/182964446-0c2c56a3-b246-4a2b-b9d0-d7dc4cf4be08.png)

- The merged LEF files are found in the `merged.lef`. It contains information about the technology and the cells in the SCL. This is an example of a DFF cell:
 ![image](https://user-images.githubusercontent.com/57360760/182964659-11209850-2bb9-41fe-b9c6-eaf42cb1a9f1.png)

 
- Finally we run the **synthesis** using `run_synthesis`: ![image](https://user-images.githubusercontent.com/57360760/182968222-db42351d-3ffa-430c-a4a2-2638289b1672.png)


- The flip flop ratio in my synthesized design is 10.8% (i.e., 1613/14876), where 1613 is the number of DFFs (`sky130_fd_sc_hd__dfxtp_2`) and 14876 is the total number of cells: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/182966275-228cc155-3c02-423e-81a3-e23858d2a132.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/182966507-e7ceb870-a8a4-4a4b-9095-bf8ade7ea2db.png)
 
- The synthesized netlist is in `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<run>/results/synthesis`:
 ![image](https://user-images.githubusercontent.com/57360760/182968718-02cedb3f-6f42-48b0-929c-127929941c3a.png)
 ![image](https://user-images.githubusercontent.com/57360760/182969643-159d296d-7f9c-4bf5-a809-248909e07008.png)
 
- The reports are in `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<run>/reports/synthesis`. In `1_yosys_4.stat.rpt` we can observe the same numbers shown above after synthesys:
 ![image](https://user-images.githubusercontent.com/57360760/182969978-22325681-72e1-440e-a313-4cba2a08777b.png)




 :question: MM I do not have the timing report he had in the video after synthesis



Synthesis (using yosys) converts the picorv32a RTL to a gate-level netlist using the skywater??? standard cell library (SCL)


## Day 2 Good floorplan vs bad floorplan and introduction to library cells

As described in Day1 there are several floorplans: chip floor-planning, macro floor-planning, power planning

- The next step after synthesis is **floorplan**. Before running the floorplan, we need to do some congiguration :question: MM- elaborate

 ![image](https://user-images.githubusercontent.com/57360760/183244330-a67fe66a-caa5-45e9-b809-0f9f8f2717b0.png)
 
 - The `README` file shows all the variables/switches required at different stages of the desing flow (e.g., `FP_CORE_UTIL` - the core utilization percentage; `FP_ASPECT_RATIO`  - the core's aspect ratio (height / width), `FP_IO_HMETAL`  - the metal layer on which to place the io pins horizontally (top and bottom of the die, default: `4`; `FP_IO_VMETAL`  - the metal layer on which to place the io pins vertically (sides of the die, default: `3`).
 -  This variables are set with their defaults in the `.tcl` files in the same directory (this files have the lowest precedence). For the chosen design (e.g., picorv32a), as mentioned in Day 1, they are set in the `config.tcl` and `sky130A_sky130_fd_sc_hd_config.tcl` files  in the `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` directory. The `sky130A_sky130_fd_sc_hd_config.tcl` has the highest precedence.

- The new content of `config.tcl` is <br/>
![image](https://user-images.githubusercontent.com/57360760/183245258-06e18032-5007-41f3-9d93-53c10e506bbf.png)

- The new content of `sky130A_sky130_fd_sc_hd_config.tcl` is <br/>
![image](https://user-images.githubusercontent.com/57360760/183256588-ddfeb0b1-b8f0-469f-8b9f-4adb1e90f16f.png) <br/>
❗Don't forget to prep the design after changing values in the config files, and rerun synthesis and floorplanning


- Since the values in `sky130A_sky130_fd_sc_hd_config.tcl` have higher precedence, the current values of this variables for the current flow,after `run_floorplan`, are: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183257433-5019afdb-9726-4440-a719-37be4f182035.png)


- The same values for the current flow can be seen in `<path_to_run>/config.tcl`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183251745-75dcb57c-5b34-47e4-895b-b8457b294792.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183257495-a7ab1fb9-6f74-4bee-b59c-f1113f6d0320.png)
 

- The area of the chip as specified in `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/06-08_10-41/results/floorplan/picorv32a.floorplan.def` is 556.4 x 567.12 microns
 ![image](https://user-images.githubusercontent.com/57360760/183257588-fe320fa4-069e-4047-88fe-c9c538bd1db1.png)

 
 - To see the layout after the floorplan, we use **magic** `<path_to>results/floorplan$ magic -T ../../../../../../../pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def` & <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183255187-be66c9e5-e11c-46f6-95f8-d3f9526ef94d.png)<br/>  
  ![image](https://user-images.githubusercontent.com/57360760/183252471-e67e04a3-9e2c-4497-aeae-fa690b348839.png)<br/>
  ![image](https://user-images.githubusercontent.com/57360760/183252550-e01c65a8-359c-4232-976c-1cf8d8614355.png)<br/>
  In these examples the shown metal layers are metal2 and metal3 forhorizontal and vertical pins (:question: shouldn't they be metal3 and metal4)

- After floorplanning, the next stage is the **placement** stage (`run_placement`). In this step, the position of the standard cells is fixed. <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183266900-66ebd8b2-0d91-4631-b941-8198beaf9ab4.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183266948-4ee4b8a8-5e58-47c4-b9f2-6f87f9838794.png)



## Day 3 Design library cell using Magic Layout and ngspice characterization

- Setting variables from within openLANE `%set ::env(FP_IO_MODE) 2` and `run_floorplan`. In magic we shoud see the pins not equdistant (i.e., stacked on top of each other) ❗MM try this and remove 
- ❗When we set variables this way, we do not have to prep the design again 

- SPICE simulation

- Clone the repo 
 ![image](https://user-images.githubusercontent.com/57360760/183284174-99ed03ee-c8b4-4f02-8149-e262ac81959f.png)
- We will be doing SPICE extraction and post-layout SPICE simulation. We first copy the `sky130A.tech` file
 ![image](https://user-images.githubusercontent.com/57360760/183284364-3ced8b22-9889-424d-b6bb-fb459d682fe9.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183284463-c8867b74-6886-4a6d-bfbc-89cf1864dfcb.png)
- We can see the layout of the invertor: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183284512-ecf1438b-4200-4614-8fdd-e160ca58c4a8.png)
- To extract it to SPICE we use the following commands in `tkcon 
 ![image](https://user-images.githubusercontent.com/57360760/183287349-5bd3c5ac-3a22-44b0-ab29-3cb1d9aa4859.png) <br/>
 - This is what the spice file currently looks like: <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183287424-1229b84f-f661-4ca9-aa67-71a9f68651f5.png) 
 - This is his file: <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183287453-374e8543-51a9-4abb-a038-d0ad369d1600.png)
  - After defining the extra connections (i.e., `Vdd, Vss, Va`, the file looks like this: <br/>
   ![image](https://user-images.githubusercontent.com/57360760/183288621-1707570b-fa93-441e-9a89-c5efa4bb630b.png)
  - When we pass this file in `ngspice` we get the folloeing output: <br/>
   ![image](https://user-images.githubusercontent.com/57360760/183288698-e6f17611-2f0a-439f-8b5b-1101c723531a.png)





   





 


## Day 4 Pre-layout timing analysis and importance of good clock tree


## Day 5 Final steps for RTL2GDS using tritonRoute and openSTA

Timing verification: Static Timing Analysis (STA) ensures that all timing constraints are met, and that the circuit runs at the designated clock frequency.
