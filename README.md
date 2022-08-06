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
 
 - The `README` file shows all the variables/switches required at different stages of the desing flow (e.g., `FP_CORE_UTIL` - the core utilization percentage; `FP_ASPECT_RATIO`  - the core's aspect ratio (height / width). This variables are set with their defaults in the `.tcl` files in the same directory (this files have the lowest precedence). For the chosen design (e.g., picorv32a), as mentioned in Day 1, they are set in the `config.tcl` and `sky130A_sky130_fd_sc_hd_config.tcl` files  in the `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` directory. The `sky130A_sky130_fd_sc_hd_config.tcl` has the highest precedence.

- The new content of `config.tcl` is <br/>
![image](https://user-images.githubusercontent.com/57360760/183245258-06e18032-5007-41f3-9d93-53c10e506bbf.png)

- The new content of `sky130A_sky130_fd_sc_hd_config.tcl` is <br/>
![image](https://user-images.githubusercontent.com/57360760/183249986-97fd374b-9c14-4da6-b65b-9aa724df4959.png)

- Since the values in `sky130A_sky130_fd_sc_hd_config.tcl` have higher precedence, the current values of this varibales after `run_floorplan` are: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183251143-5a8a9e7f-b799-4cf7-a4df-551f025af44a.png)





## Day 3 Design library cell using Magic Layout and ngspice characterization

As described in Day1 standard cells have a reular layout.


## Day 4 Pre-layout timing analysis and importance of good clock tree


## Day 5 Final steps for RTL2GDS using tritonRoute and openSTA

Timing verification: Static Timing Analysis (STA) ensures that all timing constraints are met, and that the circuit runs at the designated clock frequency.
