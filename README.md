# Advanced ASIC Development with OpenLANE/Sky130



## Application-Specific Integrated Circuits (ASICs)
**SoC design and OpenLANE**
Creating an Application-Specific Integrated Circuit (ASIC) involves the integration of three primary components:

1. **RTL IPs**: RTL (Register Transfer Level) IPs serve as the fundamental building blocks in digital design. They articulate system behavior through registers, logic gates, and the intricate flow of data between them. You can access a plethora of RTL IPs on platforms such as [github.com](https://github.com), [opencores.org](https://opencores.org), and [librecores.org](https://librecores.org).

2. **PDK Data**: The Process Design Kit (PDK) is a comprehensive resource package provided by semiconductor foundries. It equips designers with essential files, libraries, and documentation needed to craft integrated circuits (ICs) that align with specific manufacturing processes. PDKs encompass vital details regarding design regulations, device models, and technology files, all of which are indispensable for IC design and simulation.

3. **EDA Tools**: Electronic Design Automation (EDA) tools represent a category of software applications essential for crafting and evaluating electronic systems, spanning ICs and Printed Circuit Boards (PCBs). EDA tools play a pivotal role in facilitating the design and testing phases of electronic systems.

## Navigating the RTL to GDS Workflow

The journey from RTL code to a polished GDSII layout encompasses a series of critical stages:

1. **Synthesis**:
   - `yosys`: Embarks on RTL synthesis.
   - `abc`: Undertakes technology mapping.
   - `OpenSTA`: Performs static timing analysis, yielding valuable timing reports.
     ![image](https://github.com/dsingla54/pes_pd/assets/139515749/f9a2825d-5de8-4372-b91d-504266b4f054)


2. **Floorplan and Power Delivery Network (PDN)**:
   - `init_fp`: Defines the core area for the macro, rows (for placement), and tracks (for routing).
   - `ioplacer`: Strategically places macro input and output ports.
   - `pdn`: Generates the power distribution network.
   - `tapcell`: Incorporates welltap and decap cells into the floorplan.
     ![image](https://github.com/dsingla54/pes_pd/assets/139515749/2520d6be-5583-4f1e-9037-d89fe5225d95)


3. **Placement**:
   - `RePLace`: Executes global placement.
   - `Resizer`: Offers optional design optimizations.
   - `OpenPhySyn`: Delivers timing optimizations.
   - `OpenDP`: Handles detailed placement to regularize globally placed components.
![image](https://github.com/dsingla54/pes_pd/assets/139515749/50497c70-e7ba-462f-83c9-5ce0fca72826)

4. **Clock Tree Synthesis (CTS)**:
   - `TritonCTS`: Synthesizes the clock distribution network, also known as the clock tree.
![image](https://github.com/dsingla54/pes_pd/assets/139515749/5cdeb478-8af4-4d49-8d15-f99fbccb76b2)

5. **Routing**:
   - `FastRoute`: Conducts global routing, generating a guide file for the detailed router.
   - `CU-GR`: Offers an alternative for global routing.
   - `TritonRoute`: Executes detailed routing.
   - `SPEF-Extractor`: Takes care of SPEF extraction.
![image](https://github.com/dsingla54/pes_pd/assets/139515749/e9105514-0dd6-4b13-bd51-4e4354dccd9d)

6. **GDSII Generation**:
   - `Magic`: Produces the final GDSII layout file from the routed DEF.
   - `Klayout`: Provides an additional GDSII layout file as a backup.

7. **Checks**:
   - `Magic`: Carries out Design Rule Checks (DRC) and antenna checks.
   - `Klayout`: Performs DRC checks.
   - `Netgen`: Conducts Layout vs. Schematic (LVS) checks.
   - `CVC`: Executes Circuit Validity Checks.

## OpenLANE Design Stages

OpenLANE's design flow encompasses multiple stages, and you can opt for sequential or interactive execution:

### Automated Flow
To initiate the automated flow, execute the following commands:

```bash
cd OpenLane
make mount
./flow.tcl -design openlane/<DESIGN_NAME> -tag <TAG>
```

### Interactive Mode
For interactive OpenLANE usage, employ these commands:

```bash
cd OpenLane
make mount
./flow.tcl -interactive
prep -design <path_to_your_design_folder> -tag <tag> -overwrite # Overwriting is optional
```

**Interactive mode** offers us to learn all the steps present in automated flow step by step.
The steps are as follows : 

```
run_synthesis
run_floorplan
run_placement
run_cts
run_routing
write_powered_verilog followed by set_netlist $::env(lvs_result_file_tag).powered.v
run_magic
run_magic_spice_export
run_magic_drc
run_lvs
run_antenna_check
```





# Labs
<details>
<summary>DAY 1 : Inception of opensource-EDA, Openlane and Skywater130</summary>
<br>
## Skywater-130 PDK

## Getting Familiar with the Open Source EDA Tools

Tool we will be  working on pdk variant called sky130_fd_sc_hd

- sky130 : is the process name
- fd : skywater foundary
- sc : standard cell
- hd(high density) : variant of pdk

**Design Preperation step**
First we go the the working directory 
```
cd Desktop/work/tools/
cd openlane_working_dir/
cd openlane
```
Now when we  type the ```docker``` command a shell opens .
In the shell we type ```./flow.tcl -interactive```
![image](https://github.com/dsingla54/pes_pd/assets/139515749/08382953-2813-491f-a3fe-69ff6f8b2e2a)
flow.tcl is the file that contains the script to run the designs

Then we type ```package require openlane 0.9``` to import all the packages 

![image](https://github.com/dsingla54/pes_pd/assets/139515749/c7c742df-cc7e-4703-8654-cc939f68fbbd)



Now for the design setup stage, we will be working on picorv32a design.

```
prep -design picorv32a
```

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/9c6e9c0a-cbcd-472e-af24-a666bfb78444)

After preparing the design, we can see that a new 'runs' folder is created.

![image](https://github.com/dsingla54/pes_pd/assets/139515749/48bce187-63aa-41e9-b7a4-2c38aaeef88d)



Now we synthesis the design
```
run_synthesis
```

![image](https://github.com/dsingla54/pes_pd/assets/139515749/ec409133-3598-42dd-b51e-3b3c0c9f3b91)


Synthesized 

![image](https://github.com/dsingla54/pes_pd/assets/139515749/f0b4d389-40c8-4dc3-8801-9c3d89138fdd)



### Flop ratio = 1613/14876 = 0.108


![image](https://github.com/dsingla54/pes_pd/assets/139515749/a1bd0f82-5c6f-4ecd-90ff-963eeb27ff63)

</details>

<details>
<summary>DAY 2 : Good Floorplan vs Bad Floorplan and Introduction to library cells</summary>
<br>

## Floorplan

in OpenLANE, enter ```run_floorplan``` and the results will be updated in the runs folder

To view the layout of the floorplan, use the command ```magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &```


![d2_1](https://github.com/ramdev604/pes_pd/assets/43489027/b29d33fe-8860-4663-8a3e-72f0011602c3)


## Library Binding and Placement
### Placement

```run_placement```


![d2_2](https://github.com/ramdev604/pes_pd/assets/43489027/6e35d72e-789f-4026-85b6-bb02daa05741)

To view the layout of the placement, use the command ```magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &```


![d2_3](https://github.com/ramdev604/pes_pd/assets/43489027/a163cce3-dd19-4dae-9a59-271a3f96131b)


## Cell Design Flow

Cell design is done in 3 parts:

1. **Inputs** - PDKs (Process design kits), DRC & LVS rules, SPICE models, library & user-defined specs.
2. **Design Steps** - Design steps of cell design involves Circuit Design, Layout Design, Characterization. The software GUNA used for characterization. The characterization can be classified as Timing characterization, Power characterization and Noise characterization.
3. **Outputs** - Outputs of the Design are CDL (Circuit Description Language), GDSII, LEF, extracted Spice netlist (.cir), timing, noise, power.libs, function.

### Standard cell Charachterization Flow

Standard Cell Libraries consist of cells with different functionality/drive strengths. These cells need to be characterized by liberty files to be used by synthesis tools to determine optimal circuit arrangement. The open-source software GUNA is used for characterization.
Characterization is a well-defined flow consisting of the following steps:

- Link Model File of CMOS containing property definitions
- Specify process corner(s) for the cell to be characterized
- Specify cell delay and slew thresholds percentages
- Specify timing and power tables
- Read the parasitic extracted netlist
- Apply input or stimulus
- Provide necessary simulation commands

### General Timing characterization parameters

#### Timing threshold definitions

- ```slew_low_rise_thr``` - 20% from bottom power supply when the signal is rising
- ```slew_high_rise_thr``` - 20% from top power supply when the signal is rising
- ```slew_low_fall_thr``` - 20% from bottom power supply when the signal is falling
- ```slew_high_fall_thr``` - 20% from top power supply when the signal is falling
- ```in_rise_thr``` - 50% point on the rising edge of input
- ```in_fall_thr``` - 50% point on the falling edge of input
- ```out_rise_thr``` - 50% point on the rising edge of ouput
- ```out_fall_thr``` - 50% point on the falling edge of ouput

These are the main parameters that we use to calculate factors such as propogation delay and transition time

- ```propogation delay ``` - time(out_*_thr) - time(in_*_thr)
- ```Transition time``` - time(slew_high_rise_thr) - time(slew_low_rise_thr)


</details>



<details>
<summary>DAY 3 :  Design library cell using Magic Layout and ngspice characterization  </summary>
<br>


## Inverter Layout using Magic

```
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T sky130A.tech sky130_inv.mag
```

## Exploring the Layout displayed by MAGIC

Select the specific layer/device by hovering over the object and pressing, s, iteratively, until you traverse the hierarchy to the specified object:
![d3_1](https://github.com/ramdev604/pes_pd/assets/43489027/7e77d661-2c1e-4f20-93a0-09eb411a247e)


- select a region from the layout, go to the console and type ```what``` to display the information of selected area
- To select a region, place ```cursor``` on that point and  press```s```. More the number of times you press ```s```, higher the abstraction selected.




## Modified Spice netlist

![modifiedspice](https://github.com/ramdev604/pes_pd/assets/43489027/78efc81d-8f8d-4aa0-8a9c-5dd210ddbfa8)


To run the spice netlist, run ```ngspice sky130_inv.spice``` and ```plot y vs time a```
![d3_2](https://github.com/ramdev604/pes_pd/assets/43489027/375b8998-04a9-4537-af16-35ae3ba9ebc1)

![d3_3](https://github.com/ramdev604/pes_pd/assets/43489027/735909c7-fd85-4a8e-bf3f-7405d4e839d7)


The results obtained from the graph are :
- Rise Transition : 0.0395ns
- Fall transition : 0.0282ns
- Cell Rise delay : 0.03598ns
- Cell fall delay : 0.0483ns

</details>


<details>
<summary>DAY 4 : Pre-Layout timing analysis and importance of good clock tree</summary>
<br>
    
## Extraction of LEF 


Track info can be found at :

``` ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130fd_sc_hd/tracks.info```

![d4_1](https://github.com/ramdev604/pes_pd/assets/43489027/e1479fe6-55ca-4ed7-a226-2791275da645)


- 1st value indicates the offset and 2nd value indicates the pitch along provided direction

### Setting grid values using above file info

![d4_2](https://github.com/ramdev604/pes_pd/assets/43489027/1dfd759c-446f-419a-b276-aa4fa0a465bc)



- From the above pic, its confirmed that the pins A and Y are at the intersection of X and Y tracks. So the first condition is met.
- The PR boundary is taking 3 grids on width and 9 grids on height which says that the 2nd condition is also met

## LEF Generation

Since the layout is perfect, we can generate the lef file

#### 1. save the modified layout (with new grid)
   - In console, type ```save sky130_vsdinv.mag```
   - This saves the modified layout in current working directory

#### 2. Open the file and extract LEF
   - Open using ``` magic -T sky130A.tch sky130_vsdinv.mag```
   - in the console opened, type ```lef write``` and a lef file will be generated

![d4_3](https://github.com/ramdev604/pes_pd/assets/43489027/d6267e2d-62f1-4fd0-84b7-e9b3e5c8df6b)



#### 4. Make sure the lef file is added

- Include the below command to include the additional lef into the flow:
      
          set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
        
          add_lefs -src $lefs

![d4_4](https://github.com/ramdev604/pes_pd/assets/43489027/b545b775-280d-4d24-9601-845dcb073f02)


since there is slack, we have to reduce it

VLSI engineers will obtain system specifications in the architecture design phase. These specifications will determine a required frequency of operation. To analyze a circuit's timing performance designers will use static timing analysis tools (STA). When referring to pre clock tree synthesis STA analysis we are mainly concerned with setup timing in regards to a launch clock. STA will report problems such as worst negative slack (WNS) and total negative slack (TNS). These refer to the worst path delay and total path delay in regards to our setup timing restraint. Fixing slack violations can be debugged through performing STA analysis with OpenSTA, which is integrated in the OpenLANE tool. To describe these constraints to tools such as In order to ensure correct operation of these tools two steps must be taken:

- Design configuration files (.conf) - Tool configuration files for the specified design
- Design Synopsys design constraint (.sdc) files - Industry standard constraints file

For the design to be complete, the worst negative slack needs to be above or equal to 0. If the slack is outside of this range we can do one of multiple things:

1. Review our synthesis strategy in OpenLANE
    - Enalbed CELL_SIZING
    - Enabled SYNTH_STRATEGY with parameter as "DELAY 1"
    - The synthesis result is :
      

![d4_5](https://github.com/ramdev604/pes_pd/assets/43489027/f08a7a45-ce8e-4b9f-b385-c4556c3fb5a5)

    
![d4_6](https://github.com/ramdev604/pes_pd/assets/43489027/53d4f874-ccb3-4d10-b7a9-a38a798682c5)



    The delay is high when the fanout is high. Therefore we can re-run synthesis by changing the value of ```SYNTH_MAX_FANOUT``` variable
    
2. Enable cell buffering 
3. Perform manual cell replacement on our WNS path with the OpenSTA tool

    - We can see which net is driving most outputs and replace the driver cell with larger form of its own kind

    ![d4_7](https://github.com/ramdev604/pes_pd/assets/43489027/3d290a73-ce45-4909-8e78-f1383db6436f)


4. Optimize the fanout value with OpenLANE tool

Since we have synthesised the core using our vsdinv cell too and as it got successfully synthesized, it should be visible in layout after ```run_placement``` stage which is followed after ```run_floorplan``` stage
![d4_8](https://github.com/ramdev604/pes_pd/assets/43489027/4092e3cf-65e1-435f-83b5-03dc2486eb5b)

</details>

<details>
<summary>DAY 5 : Final steps for RTL2GDSII</summary>
<br>

## Power Distribution Network

After generating our clock tree network and verifying post routing STA checks we are ready to generate the power distribution network ```gen_pdn``` in OpenLANE:

The PDN feature within OpenLANE will create:

- Power ring global to the entire core
- Power halo local to any preplaced cells
- Power straps to bring power into the center of the chip
- Power rails for the standard cells

![d5_1](https://github.com/ramdev604/pes_pd/assets/43489027/d49d45aa-5d23-4e8f-930c-948c394c4af7)


Note: The pitch of the metal 1 power rails defines the height of the standard cells

## Global and Detailed Routing

OpenLANE uses TritonRoute as the routing engine ```run_routing``` for physical implementations of designs. Routing consists of two stages:

- Global Routing - Routing guides are generated for interconnects on our netlist defining what layers, and where on the chip each of the nets will be reputed
- Detailed Routing - Metal traces are iteratively laid across the routing guides to physically implement the routing guides

If DRC errors persist after routing the user has two options:

- Re-run routing with higher QoR settings
- Manually fix DRC errors specific in tritonRoute.drc file

## SPEF Extraction

After routing has been completed interconnect parasitics can be extracted to perform sign-off post-route STA analysis. The parasitics are extracted into a SPEF file. The SPEF extractor is not included within OpenLANE as of now.

```
cd ~/Desktop/work/tools/SPEFEXTRACTOR
python3 main.py <path to merged.lef in tmp> <path to def in routing>
```

The SPEF File will be generated in the location where def file is present



</details>
