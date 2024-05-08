# VSD_Digital_VLSI_SoC_Design_Workshop
### Table Of Contents
## 1.Day 1 - Inception of open-source EDA, OpenLANE and sky130 PDK
  -Introduction to all components of open-source digital asic design<br>
  -Simplified RTL2GDS flow<br />
  -Introduction to OpenLANE detailed ASIC design flow <br />

### Inception of open-source EDA, OpenLANE and sky130 PDK<br>
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

### Floorplanning and Placement

To run floorplan use the following commmand
```
run_floorplan
```
But prior running the floorplan we should check the switches related floorplan and ensure that those are correct accroding to our design requirements

![Screenshot (800)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/677603d5-d0ff-49d9-b6ce-34692284d531)

![Screenshot (801)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/24c54fda-0e69-4980-b558-40eda21419a8)

![Screenshot (802)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/d103c41f-6b46-4b45-8628-b96a49f31d7f)

After floor plan it generates .def file which has information related to floorplanning like core utilization etc

![Screenshot (805)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/418f107f-8218-46bb-b1db-cb68bd179bf1)

To view the results in GUI we will be using magic tool inside openLANE 
````
magic -T /home/vsdsquadron/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef 
  read ../../tmp/merged.lef def read picorv32a.floorplan.def &
````
![Screenshot (806)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/a02918cc-807d-4fc0-87e4-cd74b1b67cc9)
![Screenshot (808)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/46002c0e-8db4-4775-8928-b0f3c584f834)
![Screenshot (812)](https://github.com/pavang19/VSD_Digital_VLSI_SoC_Design_Workshop/assets/55171083/ced0740f-ffa5-4a96-a03d-49572dafd44a)












