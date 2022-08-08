# Advanced-Physical-Design-using-OpenLANE-Sky130-Workshop


- [Day 1. Inception of open-source EDA, openLANE and SkyWater 130](#day-1-inception-of-open-source-eda--openlane-and-skywater-130)
- [Day 2. Good floorplan vs bad floorplan and introduction to library cells](#day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
- [Day 3. Design library cell using Magic Layout and ngspice characterization](#day-3-design-library-cell-using-magic-layout-and-ngspice-characterization)
- [Day 4. Pre-layout timing analysis and importance of good clock tree](#day-4-pre-layout-timing-analysis-and-importance-of-good-clock-tree)
- [Day 5. Final steps for RTL2GDS using tritonRoute and openST](#day-5-final-steps-for-rtl2gds-using-tritonroute-and-openst)


The goal of openLANE is to produce a clean GDSII layout with no human intervention. It is a fully automated open-source end-to-end ASIC design flow. 


## Day 1. Inception of open-source EDA, openLANE and SkyWater 130

- From `‌/Desktop/work/tools/openlane_working_dir/openlane` we should start docker and execute the following commands to get into the openLANE environment:
 ![image](https://user-images.githubusercontent.com/57360760/182897041-3674f729-4e5d-4a5b-8b61-57ae81fab4ce.png)
 
- In this workshop we are using the **picorv32a** design: 
 ![image](https://user-images.githubusercontent.com/57360760/182905986-db82a812-0e88-4e9b-a9d7-da47cebe2a83.png)
 
- The variables used in the configuration files (i.e., `config.tcl` and `sky130A_sky130_fd_sc_hd_config.tcl`) are described in Day 2 and can be found in the `/Desktop/work/tools/openlane_working_dir/openlane/configuration/README.md`. At the beginning `config.tcl` has the following content: <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183244530-350722fd-ff90-4817-ad81-2b594a7a886e.png)
 
 - While `sky130A_sky130_fd_sc_hd_config.tcl` has the following content: <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183244638-9ee4e49d-33f4-44aa-b1bf-08985b3c850e.png)
 
- Afterwards we prep the design:
 ![image](https://user-images.githubusercontent.com/57360760/182964268-909ea4f1-47a2-4656-8771-a5168023a7eb.png)

- In `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` a `runs` folder is created: 
 ![image](https://user-images.githubusercontent.com/57360760/182964446-0c2c56a3-b246-4a2b-b9d0-d7dc4cf4be08.png)

- The merged LEF files are found in the `<path_to_run>/merged.lef`. It contains information about the technology and the cells in the standard cell library. This is an example of a DFF cell:
 ![image](https://user-images.githubusercontent.com/57360760/182964659-11209850-2bb9-41fe-b9c6-eaf42cb1a9f1.png)

 
- Finally we run the **synthesis** using the `run_synthesis` command. The synthesis (done using **yosys**) converts the picorv32a RTL to a gate-level netlist using the skywater standard cell library. 

- The flip flop ratio in my synthesized design is 10.8% (i.e., 1613/14876), where 1613 is the number of DFFs (`sky130_fd_sc_hd__dfxtp_2`) and 14876 is the total number of cells: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/182966275-228cc155-3c02-423e-81a3-e23858d2a132.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/182966507-e7ceb870-a8a4-4a4b-9095-bf8ade7ea2db.png)
 
- The synthesized netlist is stored in `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<run>/results/synthesis`:
 ![image](https://user-images.githubusercontent.com/57360760/182968718-02cedb3f-6f42-48b0-929c-127929941c3a.png)
 ![image](https://user-images.githubusercontent.com/57360760/182969643-159d296d-7f9c-4bf5-a809-248909e07008.png)
 
- The reports are stored in `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<run>/reports/synthesis`. In `1_yosys_4.stat.rpt` we can observe the same numbers shown above after synthesys: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/182969978-22325681-72e1-440e-a313-4cba2a08777b.png)


## Day 2. Good floorplan vs bad floorplan and introduction to library cells

- The next step after synthesis is **floorplaning**. It consist of chip floorplanning, macro floorplanning, and power planning.
- Before running the floorplan, we need to configure some variables.
 ![image](https://user-images.githubusercontent.com/57360760/183244330-a67fe66a-caa5-45e9-b809-0f9f8f2717b0.png)
 
- The `README` file shows all the variables/switches required at different stages of the desing flow (e.g., `FP_CORE_UTIL` - the core utilization percentage; `FP_ASPECT_RATIO` - the core's aspect ratio (height / width), `FP_IO_HMETAL` - the metal layer on which to place the io pins horizontally `FP_IO_VMETAL` - the metal layer on which to place the io pins vertically).
- This variables are set with their defaults in the `.tcl` files shown above (this files have the lowest precedence). For the chosen design (e.g., picorv32a), as mentioned in Day 1, the switches are set in the `config.tcl` and `sky130A_sky130_fd_sc_hd_config.tcl` files  in the `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` directory. The `sky130A_sky130_fd_sc_hd_config.tcl` has the highest precedence. (❗ Don't forget to prep the design after changing values in the config files, and rerun synthesis and floorplanning).

- The new content of `config.tcl` is: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183245258-06e18032-5007-41f3-9d93-53c10e506bbf.png)

- The new content of `sky130A_sky130_fd_sc_hd_config.tcl` is: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183256588-ddfeb0b1-b8f0-469f-8b9f-4adb1e90f16f.png) <br/>

- Since the values in `sky130A_sky130_fd_sc_hd_config.tcl` have higher precedence, the current values of this variables for the current flow, after `run_floorplan`, are: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183257433-5019afdb-9726-4440-a719-37be4f182035.png)

- The same values for the current flow can be seen in `<path_to_run>/config.tcl`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183251745-75dcb57c-5b34-47e4-895b-b8457b294792.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183257495-a7ab1fb9-6f74-4bee-b59c-f1113f6d0320.png)
 
- The area of the chip, as specified in `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/06-08_10-41/results/floorplan/picorv32a.floorplan.def`, is 556.4 x 567.12 microns.
 ![image](https://user-images.githubusercontent.com/57360760/183257588-fe320fa4-069e-4047-88fe-c9c538bd1db1.png)

 - We use **magic** to see the layout after the floorplan: <br/>
  `<path_to>results/floorplan$ magic -T ../../../../../../../pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &` <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183255187-be66c9e5-e11c-46f6-95f8-d3f9526ef94d.png)<br/>  
  ![image](https://user-images.githubusercontent.com/57360760/183252471-e67e04a3-9e2c-4497-aeae-fa690b348839.png)<br/>
  ![image](https://user-images.githubusercontent.com/57360760/183252550-e01c65a8-359c-4232-976c-1cf8d8614355.png)<br/>
  <!---In these examples the shown metal layers are metal2 and metal3 forhorizontal and vertical pins (:question: shouldn't they be metal3 and metal4)--->

- After floorplanning, the next step is the **placement** (`run_placement`). In this step, the position of the standard cells is fixed. <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183266900-66ebd8b2-0d91-4631-b941-8198beaf9ab4.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183266948-4ee4b8a8-5e58-47c4-b9f2-6f87f9838794.png)


## Day 3. Design library cell using Magic Layout and ngspice characterization

- In this part of the lab, we will use the CMOS inverter standard cell from https://github.com/nickson-jose/vsdstdcelldesign, plug it into the openlane flow, and integrate it into the pivorv32a design. <br/>
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



## Day 4. Pre-layout timing analysis and importance of good clock tree
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
 
- ❗ `prep -design picorv32a -tag <tag name of the run>` - if you want the results from the last run, or <br/>
`prep -design picorv32a -tag <tag name of the run> -overwritre` - to overwrite the last configuration with the new values in the `config.tcl` file. 

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
 `init_floorplan <br/>
 place_io <br/>
 global_placement_or <br/>
 detailed_placement <br/>
 tap_decap_or <br/>
 detailed_placement <br/>
 gen_pdn - Runs basic power grid generation on the processed design using the openroad app.` <br/>
 
- After `global_placement_or`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183318749-f9e34390-4e61-4052-93b1-8a4fda00e1e3.png)
 ❗ SLack violated, will be fixed later
- After `detailed_placement`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183318839-9d77b690-7268-4e8a-bfcc-5b6adfd437a5.png)
- After the second `detailed_placement`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183318974-31b4a8d0-48ed-453e-91b1-4393c88b0efd.png)
 
- To check whether the custom inverter cell is added to the current flow we invoke `magic` and we search for the `sky130_vsdinv` cell: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183319703-05713536-3866-430e-8b8b-1f4b6ddcb6c4.png)

- Timing analysis <br/>
- In `<path_to>/openlane` I created the `pre_sta.conf` file, on which the pre-layout static timing analysis is based. <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183381744-29db9afb-f359-433b-93b1-4ebd157d827c.png)
- After running `sta pre_sta.conf` from the  `openlane` directory, I got the following setup violations, which are same as in the last synthesis step before: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183382349-ee51baf3-e8a2-44c3-893a-63abbe00a702.png)


- In order to lower the delay, we will try to optimize the fanout values of some cells `set ::env(SYNTH_MAX_FANOUT) 4` ❓ (it was 6 before, and now it actually increased a bit): <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183389404-4b636273-0564-4091-8309-52cbda33aff8.png)

- The cell `_46535_` had a very big fanout: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183408431-fbeb3ff4-7ef8-4516-a21a-4c88a352936f.png)
 
  Or in the ~=2.9 slack`_42923_` : <br/> 
 ![image](https://user-images.githubusercontent.com/57360760/183428967-a23549eb-e010-43a2-8d34-0b8c1befbd4a.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183429376-2dfa1db9-2b40-40d6-9ea8-a73ab84e9afa.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183429698-a21bd345-f4d1-4f44-9196-85f2630216e7.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183429943-e10761e8-0353-4aef-997e-4c8badc5d500.png)


- So I tried to replace ithe following cells <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183409179-3a354e8d-3282-4423-bb63-0582884ea220.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183400592-d02b7f35-d265-4028-a889-b4a985c4ef2c.png) <br/>
 
- The slack improved a bit: >br/> 
  ![image](https://user-images.githubusercontent.com/57360760/183409416-34c4fd7c-05ae-45b5-ac11-410e653b95b5.png)

- I changed the following: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183409994-35a8a661-7464-4ee1-843c-4b94eb37423c.png) <br/>
 but nothing changed
 
- After we made these changed in OpenSTA, we should make sure that openLANE will use them: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183410981-550d1246-189f-4d53-b8be-0a6d7be0a022.png)

- To verify thatr the netlist is modified we will search for the cell `_46875_` and verify it was changed from `dfxtp_2` to `dfxtp_4`: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183419001-2915d5f6-43e5-4040-b8f6-a33dc3a344cc.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183430503-65f665a0-e278-4d56-bea9-3b2539bdfc06.png)


- Next we do floorplanning and placement using the commands mentioned before: <br/>
 - Then we `run_floorplan`. Since this command produced an error, it was suggested to use the following separate commands which give an error-free flow: <br/>
 `init_floorplan` <br/>
 `place_io` <br/>
 `global_placement_or` <br/>
 `detailed_placement` <br/>
 `tap_decap_or` <br/>
 `detailed_placement` <br/>
 `gen_pdn` <br/>
 
- After  `global_placement_or` <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183433520-b0117610-4a8d-401e-a4c3-0739122af748.png)
- After first `detailed_placement` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183433647-68dab09f-45b4-446a-96dd-b5d2f679af76.png)

- After second `detailed_placement` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183433808-f997e2de-18d1-49ad-ab77-fa9f43189894.png)

- Finaly we run the clock tree synthesis `run_cts` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183443364-a63db80e-089b-4c0d-9f7b-dc5cbf355e92.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183443900-a2ac1480-773f-4053-882b-af9f57ab8367.png)


- The next step is to do the post-cts timing analysis.
 
- We execute `openroad` in the openLANE environment and we will do the timing analysis with openSTA from there.  We read the `lef` and `def` files and create the `db` file <br/>
![image](https://user-images.githubusercontent.com/57360760/183445328-26d049b5-7e3f-4685-ad08-24e95e78da4f.png)

  
- Next we execute the command which were previosuly loaded from `pre_sta.conf` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183449034-c0e5e03f-0761-4f51-a777-a2aa6923dd4c.png) <br/>
- Hold slack <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183449630-948bb7e8-57c2-401f-a221-8d3d2d26c956.png) <br/>
- Setup slack <br/> 
 ![image](https://user-images.githubusercontent.com/57360760/183449248-7ae39b8b-a0e7-426e-ad1a-cd568cb32b2f.png) <br/>
 
- The slacks must be corrected. However, this analysis is not correct ❓ why

- We execute the following commands: <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183452090-1ce77394-a980-403f-a477-42ce6d551c42.png) <br/>
  - And this is the output: <br/> 
  - Hold slack <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183452439-18d2b90b-3d64-443c-87b2-7c851169197c.png) <br/>
  - Setup slack <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183452242-896bd805-ac8a-494e-843a-3724ddea298e.png)
 
- Both are met, the clock tree synthesis doesn't have any violationsb (but for the typicla corner, does not support the multicorner optimization ❓)


- Let's try to modify the `CTS_CLK_BUFFER_LIST` and remove the `sky130_fd_sc_hd__clkbuf_1` buffer <br/>
![image](https://user-images.githubusercontent.com/57360760/183454743-41da9f9b-58c7-4f37-bdd6-b4846ce789b8.png)
 <br/>
- We `run_cts`, but we have to forcefully stop it, since it stuck ❓!!!
 
 
- We need to do the following: <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183457820-cd7e3e2a-08e3-4bd9-bc6f-56f681f9abfc.png) <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183458124-dcaff153-f43e-4528-9902-78007430930c.png) <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183458294-c0820820-e410-4204-9aa0-2b28a3e8ea58.png)

 
- Next, we execute the same commands as before: <br/>
  ![image](https://user-images.githubusercontent.com/57360760/183459712-9481ba5f-854d-4714-962b-c01850e1c623.png)
- Hold slack <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183459883-016db9fb-4966-4bef-bd49-8e87d0e9a857.png)
- Setup slack <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183459978-33bf6e4b-948b-48f5-ad2e-09887a1990c8.png)


- Both hold and setup slacks have improved, on the expense of the area.

- Next step will be **routing** and post-routing STA. 

## Day 5. Final steps for RTL2GDS using tritonRoute and openSTA

Timing verification: Static Timing Analysis (STA) ensures that all timing constraints are met, and that the circuit runs at the designated clock frequency.
- ❗ He ran `gen_pdn` for power distribution of power and ground. But we included in the new set of commands.
- We can see the `pdn.def` created before <br/>
![image](https://user-images.githubusercontent.com/57360760/183470708-c7c547ab-f57c-44d1-afec-741d807e7337.png)

- Finally we do the routing with `run_routing`. The routing engine used is TritonRoute. <br/> 
 ![image](https://user-images.githubusercontent.com/57360760/183476372-df90ef04-1323-4c1e-8f31-0b876146b858.png)
 

- Since we used the default `ROUTING_STRATEGY`, the routing finished with 5 violation (in 64 optimization runs). In case we chose `TritinRoute = 14`, the number of violations would have been zero, on the expense of a very time consuming routing process and memory utilization. <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183479946-c7ab5256-eaf0-4164-8288-c1911a52b855.png) <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183479607-b1eff7e7-212b-4a66-ba7d-9fb6f16240ad.png)


- The violations can be manually modified in `<path_to_run>/reports/routing/tritonRoute.drc` <br/>
 ![image](https://user-images.githubusercontent.com/57360760/183479174-f8715989-a6fb-4737-b6e8-aa551d4b7cac.png)


- 

