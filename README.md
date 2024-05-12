# VSD_Digital_VLSI_SoC_Design_Workshop
### Table Of Contents
#### 1.[Inception of open-source EDA, OpenLANE and sky130 PDK](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/blob/main/README.md#1-inception-of-open-source-eda-openlane-and-sky130-pdk)
  - Introduction to all components of open-source digital asic design<br>
  - Simplified RTL2GDS flow
  - Introduction to OpenLANE detailed ASIC design flow 
  - Synthesis
#### 2.[ Floorplanning and placemnet ](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/blob/main/README.md#2-floorplanning-and-placement)
  - Steps to run floorplan using OpenLANE
  - Review floorplan files and steps to view floorplan
  - Run placemnet using openlane
#### 3. [Design library cell using Magic Layout and ngspice characterization](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/blob/main/README.md#3-design-library-cell-using-magic-layout-and-ngspice-characterization-1)
  - Labs for CMOS inverter ngspice simulations
  - Lab steps to extract spice netlist from std cell layout
  - Lab Steps to characterize the Inverter using sky130 model files
  - Lab challenge exercise to describe DRC error as geometrical construct
#### 4. [Pre-layout timing analysis and clock tree synthesis](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/blob/main/README.md#4-pre-layout-timing-analysis-and-clock-tree-synthesis-1)
  - Lab steps to convert magic layout to std cell LEF
  - Lab steps to configure synthesis settings to fix slack and include vsdinv
  - Timing Analysis using openSTA
  - Post-CTS OpenROAD timing analysis
  - Clock tree synthesis TritonCTS and signal integrity
#### 5. [Final step for RTL2GDS using tritinRoute and openSTA](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/blob/main/README.md#5-final-step-for-rtl2gds-using-tritinroute-and-opensta-1)
  - Power Distribution Network and routing
#### [References](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/blob/main/README.md#references-1)




## 1. Inception of open-source EDA, OpenLANE and sky130 PDK<br>
![Screenshot (906)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/9c016ada-0c1a-4449-baa7-9a011851d27c)

For open sourse ASIC Design we basically need three things:-<br>

1.RTL Designs-RTL (Register Transfer Level) design is a crucial methodology used for designing digital circuits.At the RTL level, the design is considered in terms of the flow of data between registers and the logic operations that take place on that data.We can get open source RTL code and open source IP's from websites shich as opencores.org librecores.org and github

2.EDA tools:- EDA (Electronic Design Automation) tools play a crucial role in the design, simulation, and layout of VLSI circuits.EDA tools are used to verify that a design will meet all the requirements of the manufacturing process.Few examples of open source EDA tools are openLANE, openROAD ,Qflow etc

3.PDK :-A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. The SkyWater Open Source PDK is a collaboration between Google and SkyWater Technology Foundry to provide a fully open source Process Design Kit and related resources, which can be used to create manufacturable designs at SkyWater’s facility.

### Simplified RTL2GDS flow<br>
![Screenshot (904)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/ca0da692-3c59-4f68-8684-fdd00aefd83b)

### Introduction to OpenLANE detailed ASIC design flow<br>

![Screenshot (905)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/a07bfd2a-5d20-42f5-8bc8-71c913bee41d)


OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. The flow performs all ASIC implementation steps from RTL all the way down to GDSII.

### Design Preparation Step
In this stage the tcl script for running the openLane flow for RTL2GDS is executed.Following commands needs to be executed
```
docker  //to enter bash
./flow.tcl -interactive // using -interactive to run step by step instead of directly RTL to GDSII
```
![Screenshot (786)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/615aa334-eafe-407c-9965-f519e5707e18) <br />

Inside the design folder there are many RTL design but for this workshop we are using picorv32a.v as RTL design source.
Now execute the following commands for design preparation:-
```
package require openlane 0.9
prep -design picorv32a
```
![Screenshot (787)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/379c1f7b-2464-48c1-bd88-6554c46c41ba)

After running this a folder named runs with subfolder with date and time gets created which will be having the details after each subsequent design steps<br />
![Screenshot (788)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/ead1bca4-aa78-46de-abb2-95e8be6343ea)

### Synthesis
For running synthesis use following command
```
run_synthesis
```
![Screenshot (792)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/56c039fc-5d4b-4220-b769-d05482904965)

After this synsthesis results and reports will get generated<br />

![Screenshot (794)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/7e1c6f9f-bb6f-485c-9916-2d8be8e7b879)

Check utilization<br />

![Screenshot (798)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/900839f7-5aaf-4e97-a15e-f827b6328961)

To Find Flipflop Ratio<br />
```
Flop ratio = Number of D FF / Total number of cells
           = 1613/14876= 0.10842 = 10.842%               
```

## 2. Floorplanning and Placement

### Steps to run floorplan using OpenLANE
To run floorplan use the following commmand
```
run_floorplan
```
But prior running the floorplan we should check the switches related floorplan and ensure that those are correct accroding to our design requirements

![Screenshot (800)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/677603d5-d0ff-49d9-b6ce-34692284d531)

![Screenshot (801)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/24c54fda-0e69-4980-b558-40eda21419a8)

![Screenshot (802)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/d103c41f-6b46-4b45-8628-b96a49f31d7f)

### Review floorplan files and steps to view floorplan

After floor plan it generates .def file which has information related to floorplanning like core utilization etc

![Screenshot (805)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/418f107f-8218-46bb-b1db-cb68bd179bf1)


To view the results in GUI we will be using magic tool inside openLANE 
````
magic -T /home/vsdsquadron/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef 
  read ../../tmp/merged.lef def read picorv32a.floorplan.def &
````
![Screenshot (806)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/a02918cc-807d-4fc0-87e4-cd74b1b67cc9)

In GUI you can see the floorplan , to zoom in press left and right mouse button and enter z to zoom in
![Screenshot (808)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/46002c0e-8db4-4775-8928-b0f3c584f834)
The tkon is the console window , select any element in the floorplan and use what comand in console to check details about it
![Screenshot (812)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/ced0740f-ffa5-4a96-a03d-49572dafd44a)
In floor plan standard cells placemnet doesnt occur , so all the standard cells can be seen at the corner

Now if some changes needs br done in floorplanning , it can be done using the switches , for eg here the io pins are placed at equi distance in this design,
if there is a requirement to plcae all the pins at once side , use this command and run floorplan again
```
set env(FP_IO_MODE) 2
```
![Screenshot (816)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/b8be14d4-3a40-47f5-8b43-02a0e4375e95)

Now all the pins are placed at the bottom side
![Screenshot (817)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/8359ed16-55e5-4433-92e5-e1d1ba445e8a)

### Run placemnet using openlane

Next step is to run the placement , to run the plcaemnet use this command
```
run_placemnet
```

![Screenshot (813)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/a17147a6-fab7-4c89-b95c-fc9762b276d5)

To check the placment in GUI , open Magic tool using this command

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```
![Screenshot (814)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/061689c9-0a9b-4d88-89f0-1b268d209d4a)
The placemnet details can be seen in the Magic tool
To select the design press S and V to bring it to the center and now press Z to zoom in to the design

![Screenshot (815)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/3f148330-4f38-4532-8d0e-06077283ebbe)

This is the zoomed view of the placement , the standard cells are all placed in the placement step

## 3. Design library cell using Magic Layout and ngspice characterization

### Labs for CMOS inverter ngspice simulations

To gitclone the given inverter files from github to your area use this command
```
 git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
![Screenshot (818)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/c0bd89cd-530e-4787-bdbc-d0bbdffab609)
Inside the vsdstdcelldesign folder , all the files realted to inverter are present 
In order to open the std cell in Magic .tech file is needed , copy the sky130A.tech file to the current folder
![Screenshot (819)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/d08617a9-bfb9-4ef4-988f-d51e02d90bfa)
Open the inverter design in the Magic tool
![Screenshot (820)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/2ea44aae-f9b1-4b64-a609-473e1fdd6e28)
To select the pmos and nmos device select the poly and diffusion layer intersection using by entering s and enter what in tkon console 
window to check if its proper or not
![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/366b364a-fefb-4977-b869-de95af3d6fc6)
![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/408e7305-9f23-4453-8d18-699081e5c2d1)

### Lab steps to extract spice netlist from std cell layout 

For characterizing this inverter in Ngspice we need the SPICE file , to ectract SPICE from the layout use these commands
```
extract all
ext2spice cthresh0 rthresh0
ext2spice
```
![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/58b91665-41da-47da-b64e-b713a4f0688b)

The SPICE file is extracted to the current directory 
![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/8ee91d8b-1cee-4d39-9983-5220ad24d5ef)

Edit the spice file to include the the power supply and the input to the cmos inverter 

![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/04762f13-6108-47b4-983e-e5f2ab7764d4)
Now to run the simulation using ngpcie use this command
```
ngspice sky130_inv.spice
```
![Screenshot (827)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/6f54886e-5c92-4e71-ad2d-210b4aedd086)

To plot the graph between Voltage and time using this command
```
plot y vs time a
```
![Screenshot (828)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/eeaa9c60-79a8-4e1c-8ba9-37316f77dced)

Increase the C3 value from 0.024ff to 2ff to decrease the ripples in the graphs

![Screenshot (830)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/049cbbcb-063e-481a-b6be-aa07d8d28aae)

### Lab Steps to characterize the Inverter using sky130 model files

- Rise Time - Rise time is typically measured from 20% to 80% of the value.<br/>
- VDD- 3.3V <br/>
- 20% of is 3.3V 660mV <br/>
- 80% of 3.3V  is 2.64V <br/>

![Screenshot (831)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/8dd65a36-ba7d-482b-ac22-021dec01d39b)

Rise time = (2.24437 - 2.18209) e-09 = 62.28ps <br/>

- Fall Time - It is the time take by output for transition from 80% to 20%.<br/>

![Screenshot (832)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/d0690da5-5fb8-443b-81a3-e8d051f43ff6)

Fall time = (4.05379-4.02) e-09 = 32.64ps <br/>

- Propagation delay-The propagation delay of a is the difference in time (calculated at 50% of input-output transition), when output switches, after application of input.<br/>

- 50% of 3.3V is 1.65<br/>

![Screenshot (834)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/6d53ed11-a3dd-4ec6-9af3-0055aceb31b6)

Propagation Delay = (2.21085 - 2.15 ) e-09 =60.85 ps <br/>

Cell Fall delay -It is the time taken for the 50% of transition from high to low at the input and low to high at the output transition <br/>

![Screenshot (835)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/dab9dd06-a2e9-4f9e-869d-b5698dbe204a)

Fall Delay = (4.07768-4.0501) e-09 = 27.58 ps <br />
### Lab introduction to Magic and steps to load Sky130 tech-rules

Download the lab files for DRC error fixing exercise using the command

```
   wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
To start magic tool,
```
  magic -d XR
```

Open met met3.mag file , different layout with different rule numbers can be seen

![Screenshot (836)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/e0e6ffaf-c68a-4c6e-8a7b-6ba266f76d6c)

![Screenshot (837)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/1a8e5c00-e865-4509-95f8-01c00aaa42e6)

select an area and fill metal3 , enter command ciff see via2 so that via mask is placed on metal 3

These rules can be checked on Google Skywater pdk documentation

![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/25762981-5a56-4800-a4a0-73a1bf816752)

### Lab challenge exercise to describe DRC error as geometrical construct

![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/145754ae-0871-4391-bd41-749d5457c316)

There are some viloations so make some changes in sky130A.tech file which are as follows:

![Screenshot (838)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/712d9541-cb84-42c4-aaf8-279ee3c1003b)

![Screenshot (839)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/02dd496d-855d-4250-bc8b-06e1b049e44d)

Now execute the commands
drc style drc(full) 
drc check

## 4. Pre-layout timing analysis and clock tree synthesis
The next process is to get the .lef file from the inverter and use that file in picorv32 design flow <br />

While designing standard cell following needs to be considered

- The Input and output ports must lie on the intersection of the Vertical and Horizontal tracks.<br />
- The width of the standard cell should be an odd multiple of the track pitch and height should be an odd multiple of track vertical pitch.<br/>

Open the tracks.info 

 ![Screenshot (840)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/1689a57f-4294-45ff-a6a1-cec84425a06d)

From this file it can be infered that for layers li1, metal 1, and metal 2, each track is positioned at (0.23, 0.46)um in the horizontal direction 
and (0.17, 0.34)um in the vertical direction.<br />

In the layout ports are on the li1 layer , translate the grid into the tracks in order to ensure that the ports are at the intersection of the tracks.<br />

To change grid into tracks first open the tracks file and then in console window type  help grid command.<br />

![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/f16c7ca0-cde8-4a15-8f49-1d1679f7e5d9) 

Now the tracks can be seen on the layout 

![Screenshot (842)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/eb1cbbf2-b393-49cf-8ead-f0b4b8e40036)

Its clear that the ports are at the intersection of the tracks

### Lab steps to convert magic layout to std cell LEF

![Screenshot (845)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/c20d2441-c301-48f1-9446-75c465a9a605)

To extract the lef file ,enter the below command in the tckon window
```
lef write
```
![Screenshot (847)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/6f32cc0f-3b89-4720-868f-164a3fe3f959)

.lef file is generated in the current directory

![Screenshot (846)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/a9985905-954e-4328-8aa2-995f6a7fc43c)

### Introduction to timing libs and steps to include new cell in synthesis

.lef file has been created, the next step is to add use this file for picorv32a<br />
For that .lef needs to be copied in src folder

![Screenshot (848)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/ab472fa8-86c4-46e1-8a2e-c76a5522e67b)

Copy the required libraries i.e .lib files to src directory 

![Screenshot (851)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/de6a0cea-61ef-46bf-b583-083bd20f6445)

Next step is to edit the config.tcl of picorv32a design to include these files

![Screenshot (852)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/171a9eb0-112a-449d-a498-5f2316fd555e)

Go to the openLANE flow and run synthesis 

for that use these commands in docker

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]      
add_lefs -src $lefs
run_synthesis
```
![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/6dc81702-f817-4e28-9f97-18335f9b826e)

![Screenshot (854)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/add30045-c1ef-4750-a696-a542441de757)

Synthesis is successful

![Screenshot (855)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/5f696a28-ac8f-4331-b686-a7a5bee1647b)

It can be seen that the inverter cell which was provided is being picked by picorv32a design during synthesis

### Lab steps to configure synthesis settings to fix slack and include vsdinv

Once the synthesis is run check the slack
![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/dfae50ea-4f59-4d45-8896-c3cc0b30cb2f)

wns= -23.89
tns== -711.59.

run the floorplan using command run_floorplan

if any errors are encountered run these commands one after another

```
init_floorplan

place_io

tap_decap_or
```
![Screenshot (859)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/a5214f98-6a43-47c8-ba65-1b80f163940a)

run the placement using command run_placement<br />

Once the placement is done open the placement in Magic tool and zoom in <br />
The standard cell inverter can be seen in the design

![Screenshot (862)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/c53de78d-4a72-4adb-ac29-9a97346e001c)

The inverter is placed and its aligned , this can be cheked using align command

### Timing Analysis using openSTA

Run the synthesis using the following commands

```
./flow.tcl -interactive

package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis

```
Create a pre_sta.conf file for STA analysis in openlane directory

![image](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/29334301-be15-4244-9b44-3581ea2ef88f)


create a my_base.sdc file which will have the constarints in the openlane/designs/picorv32a/src directory

![Screenshot (864)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/6aa659b7-9769-41f1-9731-b033e15820b0)

Now to run sta in this Desktop/work/tools/openlane_working_dir/openlane using the below command

```
sta pre_sta.conf
```
![Screenshot (867)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/5673bde9-069f-44f8-98df-bcd9c8c4ee36)
![Screenshot (866)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/31fc8e5f-3279-448a-9ed5-68b191d07498)

Here also the worst negative slack is -23.89. It can be reduced for that check the timing reports

![Screenshot (870)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/de514f96-0f51-4b16-85c5-d324e82824fc)

Look out for the nets having max fanout , then change the drive strength of corresponding gates

To check the connections to a net 
```
report net -connenctions _10566_
```
Then replace the cell with higher drive strength using this command
```
replace_cell _13165_ sky130_fd_sc_hd__or3_4
```
to check timing
```
report_checks -fields {net cap slew input_pins} -digits 4
```
Here the or gate with drive strength 2 is has fanout as 4 ,so replace the cell with the cell having drive strength as 4

![Screenshot (871)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/8f18870b-768b-417e-823c-aaabb27579eb)

Now again check  STA

![Screenshot (873)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/8fa9ef4e-8d73-4416-99bd-ec6e8db011d9)

Here it can be seen that the slack has reduced 

Repeat the same thing for other cells , the wns can still be reduced

![Screenshot (868)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/af3a1068-6eeb-45dc-bc4d-073c019b3414)

### Clock tree synthesis TritonCTS and signal integrity
Lab steps to run CTS using TritonCTS

The old netlist needs to be replaced with the new netlist in which the timing is improved ,for that first copy the current 
synthesis.v file as old as the new file will replace the current file.

To generate the new synthesis.v file use the following commad

```
write verilog  /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-05_10-43/results/synthesis/picorv32a.synthesis.v
```
![Screenshot (874)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/24a6550f-d141-4221-96aa-246d6a8755c4)

picorv32a.synthesis.v file is generated

Run the Synthesis , Floorplan and placemnet again

```
prep -design picorv32a -tag 05-05_10-43 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1
run_synthesis

init_floorplan
place_io
tap_decap_or

run_placement
```

For clock tree synthesis run this command

```
run_cts
```

![Screenshot (875)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/65b500a5-4328-468b-a73a-50d85d7823aa)


![Screenshot (877)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/e29d789f-f9e4-4aa6-ba9e-312b1a2fd1ba)

After CTS new file is generated

### Post-CTS OpenROAD timing analysis

```
openroad

read_lef /openLANE_flow/designs/picorv32a/runs/05-05_10-43/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/05-05_10-43/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/05-05_10-43/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

exit

```

![Screenshot (879)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/10ad7709-351d-425a-81b2-ba13e8fa4e27)

![Screenshot (881)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/46fe49dc-de0e-4ed5-8a36-7bd9f3f01a0e)

![Screenshot (883)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/d28f5248-49c0-49cd-9111-2419e78182b4)

Lab exercise to replace bigger CTS buffers 

first remove the clkbuf_1 from the list by using the below command
```
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
```
set the right def file and run cts
```
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/05-05_10-43/results/placement/picorv32a.placement.def
run_cts
```

To Enter the openROAD flow and check timing , use the following commands

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/05-05_10-43/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/05-05_10-43/results/cts/picorv32a.cts.def
write_db pico_cts1.db
read_db pico_cts1.db
read_verilog /openLANE_flow/designs/picorv32a/runs/05-05_10-43/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

exit

echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
echo $::env(CTS_CLK_BUFFER_LIST)

```
![Screenshot (888)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/d3080b36-542a-4162-813b-5ed159803a22)
![Screenshot (889)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/eca4ad42-7080-4ed9-aaf3-6b63426e96da)

## 5. Final step for RTL2GDS using tritinRoute and openSTA

### Power Distribution Network and routing

 After running CTS , to build power distribution network (PDN) use this command
 
 ```
gen_pdn
```
![Screenshot (893)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/b72d6fc6-3170-4311-821f-90a8fb14399e)
![Screenshot (894)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/09f661ff-61f5-4394-a91b-4b8b422d566b)

To check PDN open Magic tool go to /tmp/floorplan/ inside your runs folder and run this command
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
![Screenshot (899)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/a91bece6-9f2c-4ce1-b318-d4e00b8de467)

The power rails can be seen in this design so PDN is successful

Now to do the final step i.e routing run this command
```
run_routing
```
![Screenshot (896)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/f768b6c8-ef42-48f4-8cad-0351e11a04e1)

Routing is done with zero violations

To view the final design in Magic tool fo to the results/routing/ in your runs folder and then use this command
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```
![Screenshot (901)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/63d48021-fead-4a8a-b7b2-5ec38be84a07)
Routing is done

Final Generated Layout
![Screenshot (900)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/780b0f3e-3637-45fe-a852-0657b9b1e07f)

![Screenshot (902)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/7b3173d6-5420-4b14-8aea-0e29610a0ee7)

## References

https://github.com/google/skywater-pdk <br/>
https://github.com/nickson-jose/vsdstdcelldesign <br />
https://skywater-pdk.readthedocs.io/en/main/ <br />
https://github.com/efabless/openlane2 <br />





































































