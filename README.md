# Advanced-Physical-Design-using-OpenLANE-Sky130-Workshop

## RANDOM 

`prep -design picorv32a -tag <tag name of the run>`

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

- Clone the repo ❗Explain more
 ![image](https://user-images.githubusercontent.com/57360760/183284174-99ed03ee-c8b4-4f02-8149-e262ac81959f.png)
- We will be doing SPICE extraction and post-layout SPICE simulation. We first copy the `sky130A.tech` file, which gives all the information about the SkyWater sky130 fabrication process <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183284364-3ced8b22-9889-424d-b6bb-fb459d682fe9.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183284463-c8867b74-6886-4a6d-bfbc-89cf1864dfcb.png)
- We can see the layout of the invertor: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183284512-ecf1438b-4200-4614-8fdd-e160ca58c4a8.png)
- To extract it to SPICE we use the following commands in `tkcon` 
 ![image](https://user-images.githubusercontent.com/57360760/183287349-5bd3c5ac-3a22-44b0-ab29-3cb1d9aa4859.png) <br/>
- This is what the spice file currently looks like: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183287424-1229b84f-f661-4ca9-aa67-71a9f68651f5.png) 
- This is his file: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183287453-374e8543-51a9-4abb-a038-d0ad369d1600.png)
- After defining the extra connections (i.e., `Vdd, Vss, Va`, the file looks like this: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183288904-c6229596-ac81-4bd9-80de-c733645c2845.png)

- When we pass this file in `ngspice` we get the folloeing output: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183288698-e6f17611-2f0a-439f-8b5b-1101c723531a.png)
- The plot: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183288981-74a6e549-c74b-4718-8ae0-7cfeb46053d1.png)
 
- Library characterization of the inverter
- Next we should calculate the rise transition (20% to 80%), which appx. is `2.20-2.16 = 0.04 ns` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183289312-c5548271-3d18-442d-be73-15f442b1c0ee.png)

- Similarly, the fall transition (80% to 20%) is appx. `2.12 - 2.17 = 0.05` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183289454-f744dffa-5962-4166-abab-e96f99e86854.png)

- The cell propagation delay (cell rise delay) is appx.  `2.18 - 2.15 = 0.03` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183289577-08c324d7-3d65-4cfc-8927-dc7e0d25e22c.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183289595-2338f2a1-0884-4852-8ed3-260769530292.png) <br/>

- The cell fall delay is appx. `4.05 - 4.04 = 0.01` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183289735-52bc2165-17b0-41dd-92a9-3bac0d1c86d5.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183289759-2da4da63-5abd-43b2-b5f3-ad89b8b6ec20.png) <br/>



## Day 4 Pre-layout timing analysis and importance of good clock tree
- The first step will be to extract a `lef` file out of the magic `.mag` file <br/> and then plug that file into the picorv32a flow.

- First we need to make sure that several pconditions about the standard cell layout are met:
- (1) If we open the invertor magic file, we have to make sure the I/O of the invertor (i.e., A and Y) are on the intersection between the horizontal and vertical tracks of layer `lit1` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183309011-624c46f0-3d17-4faa-ab9b-c4a78d277c09.png) <br/>
- (2) The width of the standard cell should be in the odd multiples of the x-pitch (0.46). The same should hold true for the hight of the standard cell. 

- Then, after we configure the ports by defining their `port class` and `port use` parameters <br/>

- We generate the `lef` file using the following command: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183309719-55dc4900-1dc1-41f6-8ab0-b9e97ba41490.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183309765-ae531cf9-8e20-4647-9659-b4d0d770d61e.png) <br/>
 

- We copy the generated `lef` file to the src folder of the picorv32a design <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183309887-c04c9a65-0dce-4330-9a39-d9eec0971cfd.png) <br />
- We copy the libraries: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183310022-ed216524-1cc3-4c70-b587-5fd45349b094.png) <br />
- We modify the `config.tcl` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183311233-8a7fbb7e-3d41-47a6-a70b-62ff50bad86b.png)

 
- We prep the design <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183311139-588e08cb-ff0f-4ab0-9a47-18c22732a565.png)
 ![image](https://user-images.githubusercontent.com/57360760/183313045-20960796-72e4-4ec0-9042-dab2e23eef70.png)
- After `run_synthesis` we can see the new cell: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183313229-19bb9c27-35cc-43f0-9b25-f7e7570fbb79.png)
 as well as a slack violation ❓ (is wsn that) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183313291-24df1174-4ce1-4fcd-9cf5-f31972cb3d8f.png)

- Current values: 
`Chip area for module '\picorv32a': 147950.646400
 tns -3232.44
 wns -26.53`
 
- We changed the following switches to try to reduce the slack: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183317231-860f6d37-98f5-44c2-8af5-3314d869aac3.png)

- The new values after synthesis are (after rmoving/renaming the old `picorv32a.synthesis.v` file): <br/>
 `Chip area for module '\picorv32a': 209179.369600
 tns -266.36
 wns -2.95` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183317530-531231e7-26d3-4961-ba78-c9ec432e648b.png)



- Then we `run_floorplan`. Since this command produced an error, it was suggested to use the following separate commands which give an error-free flow: <br/>
 `init_floorplan
 place_io
 global_placement_or
 detailed_placement
 tap_decap_or
 detailed_placement
 gen_pdn` <br/>
 
- After `global_placement_or`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183318749-f9e34390-4e61-4052-93b1-8a4fda00e1e3.png)
- After `detailed_placement`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183318839-9d77b690-7268-4e8a-bfcc-5b6adfd437a5.png)
- After the second `detailed_placement`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183318974-31b4a8d0-48ed-453e-91b1-4393c88b0efd.png)
 
- To check whether the custom inverter cell is added to the current flow we invoke `magic` and we search for the `sky130_vsdinv` cell: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183319583-98aa739c-3e1c-4baf-a8fa-baf6528e3400.png)

  


 


 

 



 


## Day 5 Final steps for RTL2GDS using tritonRoute and openSTA

Timing verification: Static Timing Analysis (STA) ensures that all timing constraints are met, and that the circuit runs at the designated clock frequency.
