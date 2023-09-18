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
     ![i1](https://github.com/dsingla54/pes_pd/assets/139515749/f9a2825d-5de8-4372-b91d-504266b4f054)


2. **Floorplan and Power Delivery Network (PDN)**:
   - `init_fp`: Defines the core area for the macro, rows (for placement), and tracks (for routing).
   - `ioplacer`: Strategically places macro input and output ports.
   - `pdn`: Generates the power distribution network.
   - `tapcell`: Incorporates welltap and decap cells into the floorplan.
     ![i2](https://github.com/dsingla54/pes_pd/assets/139515749/2520d6be-5583-4f1e-9037-d89fe5225d95)


3. **Placement**:
   - `RePLace`: Executes global placement.
   - `Resizer`: Offers optional design optimizations.
   - `OpenPhySyn`: Delivers timing optimizations.
   - `OpenDP`: Handles detailed placement to regularize globally placed components.
![i3](https://github.com/dsingla54/pes_pd/assets/139515749/50497c70-e7ba-462f-83c9-5ce0fca72826)

4. **Clock Tree Synthesis (CTS)**:
   - `TritonCTS`: Synthesizes the clock distribution network, also known as the clock tree.
![i4](https://github.com/dsingla54/pes_pd/assets/139515749/5cdeb478-8af4-4d49-8d15-f99fbccb76b2)

5. **Routing**:
   - `FastRoute`: Conducts global routing, generating a guide file for the detailed router.
   - `CU-GR`: Offers an alternative for global routing.
   - `TritonRoute`: Executes detailed routing.
   - `SPEF-Extractor`: Takes care of SPEF extraction.
![i5](https://github.com/dsingla54/pes_pd/assets/139515749/e9105514-0dd6-4b13-bd51-4e4354dccd9d)

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
![d1 p1](https://github.com/dsingla54/pes_pd/assets/139515749/08382953-2813-491f-a3fe-69ff6f8b2e2a)
flow.tcl is the file that contains the script to run the designs

Then we type ```package require openlane 0.9``` to import all the packages 

![d1 p2](https://github.com/dsingla54/pes_pd/assets/139515749/c7c742df-cc7e-4703-8654-cc939f68fbbd)



Now for the design setup stage, we will be working on picorv32a design.

```
prep -design picorv32a
```

![d1 p3](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/9c6e9c0a-cbcd-472e-af24-a666bfb78444)

After preparing the design, we can see that a new 'runs' folder is created.

![d1 p4](https://github.com/dsingla54/pes_pd/assets/139515749/48bce187-63aa-41e9-b7a4-2c38aaeef88d)



Now we synthesis the design
```
run_synthesis
```

![d1 p5](https://github.com/dsingla54/pes_pd/assets/139515749/ec409133-3598-42dd-b51e-3b3c0c9f3b91)


Synthesized 

![d1 p6](https://github.com/dsingla54/pes_pd/assets/139515749/f0b4d389-40c8-4dc3-8801-9c3d89138fdd)



### Flop ratio = 1613/14876 = 0.108


![d1 p7](https://github.com/dsingla54/pes_pd/assets/139515749/a1bd0f82-5c6f-4ecd-90ff-963eeb27ff63)

</details>

<details>
<summary>DAY 2 : Good Floorplan vs Bad Floorplan and Introduction to library cells</summary>
<br>
## Strategic Considerations for Chip Floor Planning

In the intricate realm of chip design, meticulous floor planning is paramount. Here are key factors to consider when orchestrating the layout of your semiconductor masterpiece:

- **Defining Core and Die Dimensions**:
  - **Die**: The encompassing entity that constitutes the entire semiconductor chip, housing not only the core but also I/O pads and supplementary features.
  - **Core**: The central sanctum of the chip, where the bulk of the active circuitry thrives, including the CPU, GPU, memory, and assorted logic.

- **Positioning Pre-Placed Cells**:
  - **Pre-Placed Cells**: Distinctive blocks or cells, encompassing memories, clock gating cells, comparators, muxes, and more, thoughtfully positioned by the chip designer in predetermined locations prior to engaging automated placement and routing tools.

- **Incorporating Decoupling Capacitors**:
  In the realm of extensive circuits adorned with numerous resistors, challenges arise when capacitors fail to charge adequately due to voltage drops. To combat this, the employment of decoupling capacitors becomes imperative. These capacitors swiftly store and discharge electrical energy, effectively absorbing excess charge to filter out high-frequency noise and transient voltage fluctuations.

- **Strategic Power Planning**:
  During the floor planning phase, meticulous power planning is indispensable for mitigating noise in digital circuits, attributed to voltage droop and ground bounce. Transitions on a net can lead to the release of charge from coupling capacitors to the ground. To avert issues arising from insufficient ground taps, a robust Power Distribution Network (PDN) adorned with numerous power strap taps is requisite. This multiplicity of taps lowers the resistance associated with the PDN, thus enhancing performance and lowering noise.

- **Pin Placement Prowess**:
  Pin placement constitutes a pivotal facet of floor planning. It serves to minimize buffering, enhance power efficiency, and ameliorate timing delays. The High-Level Description (HDL) netlist is harnessed as a guiding star, directing the precise placement of pins within the circuit. Common pins are efficiently clustered, fostering streamlined connections, and optimizing overall performance.

In the intricate ballet of chip design, these strategic considerations for floor planning ensure that your semiconductor marvel not only meets but surpasses expectations.
## Floorplan

in OpenLANE, enter ```run_floorplan``` and the results will be updated in the runs folder

To view the layout of the floorplan, use the command ```magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &```


![d2 p1](https://github.com/dsingla54/pes_pd/assets/139515749/7389b7f2-ea92-46ce-9caa-f51df9a1b474)



## Library Binding and Placement


**Netlist Binding and Initial Placement**

Netlist binding involves mapping the logical representation of a digital design, often described in a hardware description language (HDL), onto a library of standard cells. Each component in the design is associated with a specific shape defined in the library. These shapes, along with their functionality, are part of the library. Subsequently, these components are strategically placed on the floorplan in an efficient manner to minimize signal delay.

- Components from the netlist are positioned within the core area.
- Placement is influenced by the proximity to pins for efficient signal routing.
- Strategic placement ensures swift signal propagation, especially for critical paths, with the addition of buffers to maintain signal integrity.

**Optimized Placement Using Estimated Wire-Length and Capacitance**

Estimating wire-length and capacitance is crucial for optimizing component placement, considering factors like signal delay, power consumption, and signal integrity. Large wire areas can introduce significant resistance and capacitance, potentially degrading signal quality. To address this, signals are routed through buffers to replicate and route them efficiently.

Integrating wire-length and capacitance estimates into the placement optimization process helps strike a balance between performance, power, and area considerations. The objective is to minimize signal delays, reduce power consumption, and ensure signal integrity while meeting design constraints.

**Final Placement Optimization**

Final placement optimization, coupled with timing analysis using an ideal clock, focuses on refining the physical arrangement of components in an integrated circuit while assuming perfect clock signals. This approach streamlines the physical layout without accounting for clock-related timing challenges.


```run_placement```


![d2 p2](https://github.com/dsingla54/pes_pd/assets/139515749/92c8d807-3235-4387-8560-3e6e76844bf3)


To view the layout of the placement, use the command ```magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &```


![d2 p3](https://github.com/dsingla54/pes_pd/assets/139515749/59455885-43cd-498a-bf15-e9e3c3babda3)
![image](https://github.com/dsingla54/pes_pd/assets/139515749/65bbbfea-a208-43aa-9659-facda88faec7)


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

## General timing characterization parameters

**Timing threshold definitions**

![d2 p4](https://github.com/dsingla54/pes_pd/assets/139515749/f74a6ba7-38ff-4991-9b3d-8e3796d6897d)




**Propagation Delay**
The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. 

```
Propagation delay=time(out_fall_thr)-time(in_rise_thr)
```

**Transition Time**
The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.

```
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)
```


```
Fall transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```

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
![d3 p1](https://github.com/dsingla54/pes_pd/assets/139515749/bb944d7a-9f9f-48b5-887a-8deaf74e8e3b)



- select a region from the layout, go to the console and type ```what``` to display the information of selected area
  ![d3 p2](https://github.com/dsingla54/pes_pd/assets/139515749/f931a6f1-e6b7-4b72-859b-5a9e543cbbb0)

- To select a region, place ```cursor``` on that point and  press```s```. More the number of times you press ```s```, higher the abstraction selected.
**DRC Errors**

DRC errors in magic will be highlighted with white dotted lines:
![d3 p3](https://github.com/dsingla54/pes_pd/assets/139515749/daf19ec0-5d0a-4a34-b8be-8b94de1997fa)


To identify DRC errors select DRC find next error:
it will be displayed on the tkcon window
![d3 p4](https://github.com/dsingla54/pes_pd/assets/139515749/ed39fb6f-7180-44c2-bfb2-2079b5de858f)


**Extracting to SPICE**
Command 
```
extract all
ext2spice cthresh 0 rthresh 0
```
cthresh and rthresh are used to extract all parasatic capacitances.

![d3 p5](https://github.com/dsingla54/pes_pd/assets/139515749/b7145819-7efd-4396-a4db-62812a515387)


## Modified Spice netlist

![d3 p6](https://github.com/dsingla54/pes_pd/assets/139515749/582e10a9-1e3a-4bdb-84e6-3a6154074b7b)



To run the spice netlist, run ```ngspice sky130_inv.spice``` and ```plot y vs time a```
![d3 p7](https://github.com/dsingla54/pes_pd/assets/139515749/a14ecda6-8663-4657-95e8-9226385de2ae)


![d3 p8](https://github.com/dsingla54/pes_pd/assets/139515749/4428e20e-3cd2-44a9-8f8a-be9a9e4afd45)



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

![d4 p1](https://github.com/dsingla54/pes_pd/assets/139515749/c331d8e5-897e-4308-ab09-84efe19981fd)



- 1st value indicates the offset and 2nd value indicates the pitch along provided direction

### Setting grid values using above file info

![d4 p2](https://github.com/dsingla54/pes_pd/assets/139515749/952775d5-796c-4e6b-8cbd-276e449d8978)




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
![d4 p3](https://github.com/dsingla54/pes_pd/assets/139515749/7bfa171a-8e8d-4291-9473-7b315df99587)



#### 4. Make sure the lef file is added

- Include the below command to include the additional lef into the flow:
      
          set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
        
          add_lefs -src $lefs
![d4 p4](https://github.com/dsingla54/pes_pd/assets/139515749/d910e345-7f01-4753-92a6-1ce5eec05a65)



since there is slack, we have to reduce it

VLSI engineers will obtain system specifications in the architecture design phase. These specifications will determine a required frequency of operation. To analyze a circuit's timing performance designers will use static timing analysis tools (STA). When referring to pre clock tree synthesis STA analysis we are mainly concerned with setup timing in regards to a launch clock. STA will report problems such as worst negative slack (WNS) and total negative slack (TNS). These refer to the worst path delay and total path delay in regards to our setup timing restraint. Fixing slack violations can be debugged through performing STA analysis with OpenSTA, which is integrated in the OpenLANE tool. To describe these constraints to tools such as In order to ensure correct operation of these tools two steps must be taken:

- Design configuration files (.conf) - Tool configuration files for the specified design
- Design Synopsys design constraint (.sdc) files - Industry standard constraints file

For the design to be complete, the worst negative slack needs to be above or equal to 0. If the slack is outside of this range we can do one of multiple things:

1. Review our synthesis strategy in OpenLANE
    - Enalbed CELL_SIZING
    - Enabled SYNTH_STRATEGY with parameter as "DELAY 1"
    - The synthesis result is :
      

![d4 p5](https://github.com/dsingla54/pes_pd/assets/139515749/d5aaf76e-5580-487e-8bca-b76dbfaccdd6)


    
![d4 p6](https://github.com/dsingla54/pes_pd/assets/139515749/cedbad61-8fa8-40dd-8786-6dfb480109e4)




    The delay is high when the fanout is high. Therefore we can re-run synthesis by changing the value of ```SYNTH_MAX_FANOUT``` variable
    
2. Enable cell buffering 
3. Perform manual cell replacement on our WNS path with the OpenSTA tool

    - We can see which net is driving most outputs and replace the driver cell with larger form of its own kind

 ![d4 p7](https://github.com/dsingla54/pes_pd/assets/139515749/8b9805b0-b8a1-4064-a09b-54636d3df1e7)



4. Optimize the fanout value with OpenLANE tool

Since we have synthesised the core using our vsdinv cell too and as it got successfully synthesized, it should be visible in layout after ```run_placement``` stage which is followed after ```run_floorplan``` stage
![d4 p8](https://github.com/dsingla54/pes_pd/assets/139515749/04791844-43fc-4ac9-94b6-57369d16565f)


</details>

<details>
<summary>DAY 5 : Final steps for RTL2GDSII</summary>
<br>

## Power Distribution Network

PDN (Power Delivery Network) routing is a crucial aspect of integrated circuit design. It involves the creation of a network of traces and components to ensure that power is distributed effectively and reliably to all parts of the electronic device. 

Global and detailed routing are two essential steps in the design and manufacturing of integrated circuits. 
After generating our clock tree network  we  generate the power distribution network gen_pdn using  OpenLANE:

The PDN  will create:

- Power ring global for the entire core
A global power ring is a continuous metal ring that surrounds the entire core of the IC.It's used to distribute power (VDD) uniformly to the core logic and various functional blocks.The power ring ensures that all regions of the core receive power without significant voltage drops.

- Power halo local to any preplaced cells
A power halo is a localized power distribution network around specific preplaced cells or macroblocks on the chip.Preplaced cells are often fixed in their positions, and a power halo provides them with the necessary power connections.

- Power straps to bring power into the center of the chip
Power straps are metal traces or structures used to bring power from the periphery of the chip towards the central regions.They are essential for delivering power to the core logic and other critical areas, reducing the distance power must travel.Power straps help maintain uniform power distribution across the chip.

- Power rails for the standard cells
Power rails are metal lines that run vertically or horizontally across the chip, supplying power to standard cells .These power rails ensure that each standard cell has access to the power it needs for proper operation.

```gen_pdn```
![d5 p1   ](https://github.com/dsingla54/pes_pd/assets/139515749/4356d7e0-d23b-4b6c-8ccc-7cd3bdbbc518)
![d5 p2](https://github.com/dsingla54/pes_pd/assets/139515749/75b66a01-77bf-485a-a87c-5876ebf41a99)

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
