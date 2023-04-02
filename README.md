# VSD Mixed-signal PD Research Program - msvsdqwc

# Table of Contents  
 - [Week 1](https://github.com/Haritha266/msvsdqwc#week--1)
 - [Week 2](https://github.com/Haritha266/msvsdqwc#week--2)
 - [Week 3](https://github.com/Haritha266/msvsdqwc#week--3)
 - [Week 4](https://github.com/Haritha266/msvsdqwc#week--4)
 - [Week 5](https://github.com/Haritha266/msvsdqwc#week--5)
 - [Week 6](https://github.com/Haritha266/msvsdqwc#week--6)
 - [Week 7](https://github.com/Haritha266/msvsdqwc#week--7)
 
 # Week -1
 
 ## Install Oracle virtual box with Ubuntu 20.04
 - If you have a windows machine, install Oracle virtual box with Ubuntu 20.04 - RAM at least 4GB, hard-disk atleast 50GB 
 - Watch first 5 lectures of [VSD Open source tool installation course](https://www.udemy.com/course/vsd-a-complete-guide-to-install-open-source-eda-tools/learn/lecture/6719216#overview) to know installation procedure
 - Refer to [repo](https://github.com/kunalg123/vsdflow) for installation steps
 - Open terminal in virtual box and install git using `sudo apt-get install git`


 ## Install magic and SKY130 PDKs
 - Refer to chapter 0 of [PV GitHub repo](https://github.com/sanampudig/OpenFASoC/tree/main/AUXCELL)
 - Follow the below commands to install magic
  ```
$ git clone git://opencircuitdesign.com/magic
$ cd magic
$ sudo apt-get install m4
$ sudo apt-get install tcsh
$ sudo apt-get install csh
$ sudo apt-get install libx11-dev
$ sudo apt-get install tcl-dev tk-dev
$ sudo apt-get install libcairo2-dev
$ sudo apt-get install mesa-common-dev libglu1-mesa-dev
$ sudo apt-get install libncurses-dev
$ ./configure
$ make
$ sudo make install
```
More info can be found at [magic](http://opencircuitdesign.com/magic/index.html)

- To install SKY130  PDKs
## Netgen
Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic" <br /><br />
Install steps:
```
$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$	./configure
$  make
$  sudo make install
```
More info can be found at [http://opencircuitdesign.com/netgen/index.html](http://opencircuitdesign.com/netgen/index.html)

## Xschem
Xschem is a schematic capture program
Install steps:
```
$ git clone https://github.com/StefanSchippers/xschem.git xschem_git
$ sudo apt-get install flex
$ sudo apt-get install bison
$ sudo apt-get install libxpm-dev
$ ./configure
$ make
$ sudo make install
```
More info can be found at [http://repo.hu/projects/xschem/index.html](http://repo.hu/projects/xschem/index.html)

## Ngspice
ngspice is the open-source spice simulator for electric and electronic circuits.
'sudo apt-get install ngspice'
More info can be found at [https://ngspice.sourceforge.io/index.html](https://ngspice.sourceforge.io/index.html)

Please note that to view the simulation graphs of ngspice, xterm is required and can be installed using.
```
$ sudo apt-get update
$ sudo apt-get install xterm
```
## open_pdk

Open_PDKs is distributed with files that support the Google/SkyWater sky130 open process description [https://github.com/google/skywater-pdk](https://github.com/google/skywater-pdk). Open_PDKs will set up an environment for using the SkyWater sky130 process with open-source EDA tools and tool flows such as magic, qflow, openlane, netgen, klayout, etc.<br /><br />
Install steps:

```
$  git clone git://opencircuitdesign.com/open_pdks
$  cd open_pdks
$	./configure --enable-sky130-pdk
$  make
$  sudo make install
```
## Create a lab directory folder with name inverter on desktop to perform the experiments

```
$ mkdir inverter
$ cd inverter
$ mkdir mag
$ mkdir netgen
$ mkdir xschem
$ cd xschem
$ cp /usr/local/share/pdk/sky130A/libs.tech/xschem/xschemrc .
$ cp /usr/local/share/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit
$ cd ../mag
$ cp /usr/local/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
$ cd ../netgen
$ cp /usr/local/share/pdk/sky130A/libs.tech/netgen//sky130A_setup.tcl .
```
 ## Create inverter and perform pre-layout using xschem or ngspice
 
 Create an inverter schematic using xschem by taking pfet and nfet from sky130 PDK and other components from devices
 
 ![0](https://user-images.githubusercontent.com/83575446/219820916-7fd332ea-fdbd-4aa6-884f-fab441bd3543.png)
 
 Create symbol from schematic of inverter

 ![01](https://user-images.githubusercontent.com/83575446/219820984-e0c35c21-5c58-4e9f-a23c-03f07582e143.png)

Generate netlist for pre-layout of inverter and plot the graphs

  The following path has to be added to add sky130 PDK technology
  `.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt`
  
![1](https://user-images.githubusercontent.com/83575446/219821214-9c4bf545-06fe-49a8-9c6a-36fda028615b.png)

  Generate the netlist and done the simulation

![2](https://user-images.githubusercontent.com/83575446/219821451-c29c8307-73b6-4923-98cc-994e5f7288d6.png)

## Inverter Post-layout characterization using magic and SKY130 PDKs

Go to the working directory and enter the following commands

```
magic -T sky130A.magicrc
magic -T sky130A.tech
```

![3](https://user-images.githubusercontent.com/83575446/219821583-c82c62ee-89a1-4380-a76c-f0f1d46365b9.png)

In the tkcon window check for the DRC errors and perform post layout charectarization

```
extract do local
extract all
ext2spice lvs
ext2spice cthresh 0 rthresh 0
ext2spice
```
This will create inverter.mag and inverter.ext files and also a .spice netlist. This .spice netlist generated post layout contains the parasitics that were absent in pre-layout netlist.Now selectively paste the control statements into postlayout netlist extracted from Magic Tool

![4](https://user-images.githubusercontent.com/83575446/219821606-efc30934-0b16-4cad-87f0-7b74ec31652a.png)

## Creting schematic for the function  Fn = [(B+D).(A+C)+E.F]' and perform pre-layout using xschem or ngspice

![8](https://user-images.githubusercontent.com/83575446/219823135-3bd8530e-189d-4a42-a12f-b723bf9c280d.png)

Generate netlist for pre-layout of Fn and plot the graphs

  The following path has to be added to add sky130 PDK technology
  `.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt`

![9](https://user-images.githubusercontent.com/83575446/219823200-80005e32-377c-4dc1-8485-22dbf56b8664.png)

# Week -2

## Installing ALIGN:
**Prerequisites**

- gcc >= 6.1.0 (For C++14 support)
- python >= 3.7 

```
export CC=/usr/bin/gcc
export CXX=/usr/bin/g++
git clone https://github.com/ALIGN-analoglayout/ALIGN-public
cd ALIGN-public

#Create a Python virtualenv
python3.8 -m venv general
source general/bin/activate
python3.8 -m pip install pip --upgrade

# Install ALIGN as a USER
pip install -v .

# Install ALIGN as a DEVELOPER
pip install -e .

pip install setuptools wheel pybind11 scikit-build cmake ninja
pip install -v -e .[test] --no-build-isolation
pip install -v --no-build-isolation -e . --no-deps --install-option='-DBUILD_TESTING=ON'

```

## Making ALIGN Portable to Sky130 tehnology

Clone the following Repository inside ALIGN-public directory

```
git clone https://github.com/ALIGN-analoglayout/ALIGN-pdk-sky130
```

move `SKY130_PDK` folder to `/home/haritha266/Desktop/ALIGN-public/pdks`

## Running ALIGN TOOL

Everytime we start running tool in new terminal run following commands.

```
python3.8 -m venv general
source general/bin/activate
```
Commands to run ALIGN (goto ALIGN-public directory)

```
mkdir work
cd work
```
General syntax to give inputs
```
schematic2layout.py <NETLIST_DIR> -p <PDK_DIR> -c
```
Create .lef and .gds files for inverter using the following SPICE Netlist with .sp file extension

 ```
.subckt inverter vdd vin vout vss
XM1 vout vin vdd vdd sky130_fd_pr__pfet_01v8 L=0.15 W=2.6 nf=1 
XM2 vout vin vss vss sky130_fd_pr__nfet_01v8 L=0.15 W=1 nf=1 
.ends inverter
```

In the working directory give the user inputs

`schematic2layout.py /home/haritha266/Desktop/inverter -p /home/haritha266/Desktop/ALIGN-public/pdks/SKY130_PDK/`


![10](https://user-images.githubusercontent.com/83575446/219824165-1255f4fc-41d6-4987-8ca7-1a55cfeea9ff.png)

Using klayout, .gds and .lef files can be viewed

![5](https://user-images.githubusercontent.com/83575446/219824714-00d16016-4fa7-4efb-b24a-4adbb54a640d.png)


## Post layout characterization of inverter using ALIGN
Magic Tool is used to post layout Spice file. SPICE file can be simulated in NGSPICE and compare with prelayout.

- Open terminal in work directory where our final gds stored.

set PDK ROOT for Magic using the command -
```
export PDK_ROOT=/path/to/your/pdks/
```

Then type `magic` in terminal which open magic.

- Then goto file and press read GDS and select our gds file
- Place the curser outside layout press `s` which select entire layout.
- Then goto tkcon and type `ext2spice`
- post layout spice file is created in work directory

![6](https://user-images.githubusercontent.com/83575446/219824391-bad75b41-ef70-424a-979e-917d362ee7e2.png)

This .spice netlist generated post layout contains the parasitics that were absent in pre-layout netlist.

![7](https://user-images.githubusercontent.com/83575446/219824786-d9e767ca-67a9-425d-9e8e-90ded7836766.png)

## Function Post-layout characterization using magic and SKY130 PDKs

Go to the working directory and enter the following commands

```
magic -T sky130A.magicrc
magic -T sky130A.tech
```

![11](https://user-images.githubusercontent.com/83575446/221121024-0220028a-c127-4c61-829a-b9efe91e4abe.png)

In the tkcon window check for the DRC errors and perform post layout charectarization

```
extract do local
extract all
ext2spice lvs
ext2spice cthresh 0 rthresh 0
ext2spice
```
This .spice netlist generated post layout contains the parasitics that were absent in pre-layout netlist.Now selectively paste the control statements into postlayout netlist extracted from Magic Tool

![12](https://user-images.githubusercontent.com/83575446/221121048-6a540de2-5756-4e0c-b3e5-1588417f924b.png)

## Post layout characterization of Fn using ALIGN

In the working directory of ALIGN give the user inputs

`schematic2layout.py /home/haritha266/Desktop/function -p /home/haritha266/Desktop/ALIGN-public/pdks/SKY130_PDK/`

Using klayout, .gds and .lef files can be viewed

![13](https://user-images.githubusercontent.com/83575446/221121068-b949a4bb-d6d3-4388-bc5c-ea68f749d755.png)

Magic Tool is used to post layout Spice file. SPICE file can be simulated in NGSPICE and compare with prelayout.

- Open terminal in work directory where our final gds stored.

set PDK ROOT for Magic using the command -
```
export PDK_ROOT=/path/to/your/pdks/
```

Then type `magic` in terminal which open magic.

- Then goto file and press read GDS and select our gds file
- Place the curser outside layout press `s` which select entire layout.
- Then goto tkcon and type `ext2spice`
- post layout spice file is created in work directory

![14](https://user-images.githubusercontent.com/83575446/221120951-0f889ad4-f1a0-4dd9-8323-5916906c5888.png)

![15](https://user-images.githubusercontent.com/83575446/221120963-4929f376-0e83-4d03-94b5-671b5376161d.png)

This .spice netlist generated post layout contains the parasitics that were absent in pre-layout netlist.

![16](https://user-images.githubusercontent.com/83575446/221120969-e2a20933-ffe0-40cb-966f-91d6d67906b4.png)

# Week -3

## Intalling OpenROAD

OpenROAD is an integrated chip physical design tool that takes a design from synthesized Verilog to routed layout.

An outline of steps used to build a chip using OpenROAD is shown below:

- Initialize floorplan - define the chip size and cell rows
- Place pins (for designs without pads )
- Place macro cells (RAMs, embedded macros)
- Insert substrate tap cells
- Insert power distribution network
- Macro Placement of macro cells
- Global placement of standard cells
- Repair max slew, max capacitance, and max fanout violations and long wires
- Clock tree synthesis
- Optimize setup/hold timing
- Insert fill cells
- Global routing (route guides for detailed routing)
- Antenna repair
- Detailed routing
- Parasitic extraction
- Static timing analysis

## Steps to install openROAD are

```
cd
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD.git
cd OpenROAD
sudo ./etc/DependencyInstaller.sh
cd
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
./build_openroad.sh –local
export OPENROAD=~/OpenROAD-flow-scripts/tools/OpenROAD
export PATH=~/OpenROAD-flow-scripts/tools/install/OpenROAD/bin:~/OpenROAD-flow-scripts/tools/install/yosys/bin:~/OpenROAD-flow-scripts/tools/install/LSOracle/bin:$PATH
```
## Intalling OpenFASOC

OpenFASoC is a project focused on automated analog generation from user specification to GDSII with fully open-sourced tools. It is led by a team of researchers at the University of Michigan and is inspired from FASoC which sits on proprietary software.

The tool is comprised of analog and mixed-signal circuit generators, which automatically create a physical design based on user specifications.


- First, cd into a directory of your choice and clone the OpenFASoC repository:

`git clone https://github.com/idea-fasoc/openfasoc`

- Now go to the home location of this repository (where the README.rst file is located) and run `sudo ./dependencies.sh.`
- Or follow the below steps
```
cd
git clone https://github.com/idea-fasoc/openfasoc
cd openfasoc
sudo ./dependencies.sh

```
# Temperature Sensor Auxiliary Cells
An overview of how the Temperature Sensor Generator (temp-sense-gen) works internally in OpenFASoC

## Circuit
This generator creates a compact mixed-signal temperature sensor based on the topology from this paper. It consists of a ring oscillator whose frequency is controlled by the voltage drop over a MOSFET operating in subthreshold regime, where its dependency on temperature is exponential.

<p align="center">
  <img width="1000" height="500" src="https://user-images.githubusercontent.com/110079890/200105228-1ba0d839-3f1a-482f-9889-bb2cd1eb5048.png">
</p>

The physical implementation of the analog blocks in the circuit is done using two manually designed standard cells:

1. HEADER cell, containing the transistors in subthreshold operation;

2. SLC cell, containing the Split-Control Level Converter.

The gds and lef files of HEADER and SLC cells are pre-created before the start of the Generator flow.

The layout of the HEADER cell is shown below:
<p align="center">
  <img width="1000" height="500" src="https://user-images.githubusercontent.com/110079890/207322258-561ed913-58b6-433c-bb46-4d8772cb8845.png">
</p>

The layout of the SLC cell is shown below:
<p align="center">
  <img width="1000" height="500" src="https://user-images.githubusercontent.com/110079890/207323939-51a7531f-f1df-4e8f-af5f-6ae798c07a6c.png">
</p>


## OpenFASOC flow for Temperature Sensor Generation
To configure circuit specifications, modify the test.json specfile in the generators/temp-sense-gen/ folder.

To run the default generator, cd into openfasoc/generators/temp-sense-gen/ and execute the following command:

`make sky130hd_temp`

The default circuit’s physical design generation can be divided into three parts:

1. Verilog generation

2. RTL-to-GDS flow (OpenROAD)

3. Post-layout verification (DRC and LVS)

### Verilog Generation
To run verilog generation, type the command
`make sky130hd_temp_verilog`

The generator must first parse the user’s requirements into a high-level circuit description or verilog. User input parsing is implemented by reading from a JSON spec file directly in the temp-sense-gen repository. The JSON allows for specifying power, area, maximum error (temperature result accuracy), an optimization option (to choose which option to prioritize), and an operating temperature range (minimum and maximum operating temperature values). The operating temperature range and optimization must be specified, but other items can be left blank.

The generator uses this model file to automatically determine the number of headers and slc, among other necessary modifications that can be made to meet spec. The generator references the model file in an iterative process until either meeting spec or failing. A verilog description is then produced by substituting specifics into template verilog files.

The test.json file shown in the below screenshot corresponds to the temp_sense_gen.

<p align="center">
  <img width="1000" height="500" src="https://user-images.githubusercontent.com/110079890/200110700-17034822-61a6-4b29-aee5-09888bcde8ae.png">
</p>

In this file temperature is being varied from -20 C to 100 C.Based on the operating temperature range, generator calculates the number of header and inverters to minimize the error.

The generator starts from a Verilog template of the temperature sensor circuit, located in temp-sense-gen/src/. The .v template files have lines marked with @@, which are replaced according to the specifications.

To replace these lines with the correct circuit elements, temp-sense-gen takes cells from the selected technology. The number of inverters in the ring oscillator and of HEADER cells in parallel are optimized using a nearest-neighbor approach with experimental data from models/modelfile.csv.

The generator references the model file in an iterative process until either meeting specifications or failing.
<p align="center">
  <img width="1000" height="500" src="https://user-images.githubusercontent.com/110079890/207328599-f40c124e-06cc-4128-a0f8-1ab3808210be.png">
</p>

As shown in the above picture, the tool is trying to minimize the error iteratively, by varying the number of inverters and headers for the given temperature range.

#### Directory where verilog files are generated
<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/201682391-8468fd05-dfe6-408e-a94d-54da7b8c6a2c.png">
</p>
After running the `make sky130hd_temp_verilog` command the verilog files of counter.v, TEMP_ANALOG_hv.nl.v, TEMP_ANALOG_lv.nl.v are created in the src folder.

Here, using the generic template, extra blocks of counter, TEMP_ANALOG_hv.nl.v, TEMP_ANALOG_lv.nl.v are created in the src folder. For the temperature specification of -20C to 100C, we see that three header files are required.

### Synthesis

The OpenROAD Flow starts with a flow configuration file config.mk, the chosen platform (sky130hd, for example) and the Verilog files are generated from the previous part.

The config.mk file is shown below:
<p align="center">
  <img width="1000" height="1000" src="https://user-images.githubusercontent.com/110079890/207945871-6812a6ed-9e19-48c8-8ea0-6b6689a6d9a0.png">
</p>

The synthesis is run using Yosys to find the appropriate circuit implementation from the available cells in the platform. The synthesized verilog netlist is saved as shown in the below figure.
<p align="center">
  <img width="2000" height="400" src="https://user-images.githubusercontent.com/110079890/207364136-3c0d4fe7-7620-4b9c-9329-225b65925228.png">
</p>

### Floorplan
The floorplan for the physical design is generated with OpenROAD, which requires a description of the power delivery network in pdn.cfg.

The floorplan final power report is shown below:
<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/207365893-86f050f0-037f-4f2f-822f-78f02161d825.png">
</p>

This temperature sensor design implements two voltage domains: one for the VDD that powers most of the circuit, and another for the VIN that powers the ring oscillator and is an output of the HEADER cells. Such voltage domains are created within the floorplan.tcl script, with the following lines of code:

<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/207369337-441cabe9-8ff6-463b-ab31-ca984f2d6061.png">
</p>

In the image, line #34 will create a voltage domain named TEMP_ANALOG with area coordinates as defined in config.mk.

Lines #36 to #38 will initialize the floorplan, as default in OpenROAD Flow, from the die area, core area and place site coordinates from config.mk.

And finally, lines #40 to #42 will source read_domain_instances.tcl, a script that assigns the corresponding instances to the TEMP_ANALOG domain group. It gets the wanted instances from the DOMAIN_INSTS_LIST variable, set to tempsenseInst_domain_insts.txt in config.mk. This will ensure the cells are placed in the correct voltage domain during the detailed placement phase.

The tempsenseInst_domain_insts.txt file contains all instances to be placed in the TEMP_ANALOG domain (VIN voltage tracks). These cells are the components of the ring oscillator, including the inverters whose quantity may vary according to the optimization results. Thus, this file actually gets generated during temp-sense-gen.py.

### Placement
Placement takes place after the floorplan is ready and has two phases: global placement and detailed placement. The output of this phase will have all instances placed in their corresponding voltage domain, ready for routing.

#### The Global Placement power and area report is shown below:
<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/207367846-cd120f44-d050-44ab-ad64-b3be19542531.png">
</p>

#### The Detail Placement power and area report is shown below:
<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/207370865-49874446-01d1-499b-9ba3-0b531b416b04.png">
</p>

### Routing
Routing is also divided into two phases: global routing and detailed routing. Right before global routing, OpenFASoC calls pre_global_route.tcl:
<p align="center">
  <img width="1000" height="500" src="https://user-images.githubusercontent.com/110079890/207374779-15b542d2-9b53-4b6d-9870-5c63a7b8d2fe.png">
</p>

This script sources two other files: add_ndr_rules.tcl, which adds an NDR rule to the VIN net to improve routes that connect both voltage domains, and create_custom_connections.tcl, which creates the connection between the VIN net and the HEADER instances.

#### The Global route power and area report is shown below:
<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/207372763-4d811680-c1f7-432d-84e8-b3c7a6f7e8cf.png">
</p>

#### The finished power and area report is shown below:
<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/207377437-e6d78adc-05cb-476f-8751-db3d731d5dc7.png">
</p>

#### Final layout after routing:

<p align="center">
  <img width="700" height="500" src="https://user-images.githubusercontent.com/110079890/207379770-e9cb8f5f-b8ed-4ace-880e-266944286578.png">
</p>


### Post-layout verification

After generating the design, OpenFASoC runs DRC and LVS to check that the circuit is manufacturable and corresponds to the specified design. In flow/Makefile, the targets magic_drc and netgen_lvs are run using make.
In DRC, Magic takes the generated GDS file and checks for failed constraints. A report is written under temp-sense-gen/flow/reports/ with any errors found.

In LVS, Magic takes the generated GDS file and extracts its netlist to compare with the original circuit netlist, in order to verify if the physical implementation was done correctly. Files generated from the layout extraction are created under temp-sense-gen/flow/objects/.

Netgen is then used to run the comparison, outputting a report under temp-sense-gen/flow/reports/.

![17](https://user-images.githubusercontent.com/83575446/221120978-28a89ff2-66b3-4ee1-9f88-28302f48213a.png)

# Week -4
 
 ## Circuit diagram of 4-Bit Asynchronous Up Counter
 
 ![image](https://user-images.githubusercontent.com/83575446/222718876-f0587a88-4bea-47db-81c7-81cf1c3d421f.png)

## Implementation of ring oscillator in Xschem

![19](https://user-images.githubusercontent.com/83575446/222718500-dd394bd3-ec73-47f4-83f2-11a8fca959a9.png)

![18](https://user-images.githubusercontent.com/83575446/222718518-e89efcbd-27af-4298-b224-3f7d777ee82d.png)

## Post layout characterization of ring oscillator using ALIGN

In the working directory of ALIGN give the user inputs

`schematic2layout.py /home/haritha266/Desktop/analog -p /home/haritha266/Desktop/ALIGN-public/pdks/SKY130_PDK/`

![20](https://user-images.githubusercontent.com/83575446/222718552-d3109352-d58c-489c-a15f-1b784e755716.png)

Using klayout, .gds and .lef files can be viewed

![21](https://user-images.githubusercontent.com/83575446/222719801-7153cfde-7f72-4356-8f08-34802b11f1db.png)

![22](https://user-images.githubusercontent.com/83575446/222718408-ad240265-af39-4d5b-87be-16044b1f8e0c.png)

Magic Tool is used to post layout Spice file. SPICE file can be simulated in NGSPICE and compare with prelayout.

- Open terminal in work directory where our final gds stored.

set PDK ROOT for Magic using the command -
```
export PDK_ROOT=/path/to/your/pdks/
```

Then type `magic` in terminal which open magic.

- Then goto file and press read GDS and select our gds file
- Place the curser outside layout press `s` which select entire layout.
- Then goto tkcon and type `ext2spice`
- post layout spice file is created in work directory

![23](https://user-images.githubusercontent.com/83575446/222718413-c39fc022-c255-4b56-af95-390d93643fc4.png)

![24](https://user-images.githubusercontent.com/83575446/222718420-bc2fb29d-9321-46c0-aa21-a5e54b232fc1.png)

This .spice netlist generated post layout contains the parasitics that were absent in pre-layout netlist.

![25](https://user-images.githubusercontent.com/83575446/222718426-5dea3690-ce67-4168-be4c-e8d0561dbdd1.png)

# Week -5
 
 ## Circuit diagram of ADC
 
 ![26](https://user-images.githubusercontent.com/83575446/224398125-806702e9-c82d-410f-8171-624d364c876c.png)

## Implementation of ADC in Xschem

![27](https://user-images.githubusercontent.com/83575446/224398339-dd02f181-6c9d-4d83-9a25-4ad2a8cc1d50.png)

## Post layout characterization of ADC using ALIGN

In the working directory of ALIGN give the user inputs

`schematic2layout.py /home/haritha266/Desktop/analog -p /home/haritha266/Desktop/ALIGN-public/pdks/SKY130_PDK/`

Using klayout, .gds and .lef files can be viewed

![29](https://user-images.githubusercontent.com/83575446/224399088-58eaa280-1685-487b-91db-85e9a06fc26e.png)

![30](https://user-images.githubusercontent.com/83575446/224399105-48639ac8-3ddf-4462-9894-324e19057c1f.png)

Magic Tool is used to post layout Spice file. SPICE file can be simulated in NGSPICE and compare with prelayout.

- Open terminal in work directory where our final gds stored.

set PDK ROOT for Magic using the command -
```
export PDK_ROOT=/path/to/your/pdks/
```

Then type `magic` in terminal which open magic.

- Then goto file and press read GDS and select our gds file
- Place the curser outside layout press `s` which select entire layout.
- Then goto tkcon and type `ext2spice`
- post layout spice file is created in work directory

![28](https://user-images.githubusercontent.com/83575446/224398994-a3851fa0-8358-4623-867d-f16b233aac32.png)

This .spice netlist generated post layout contains the parasitics that were absent in pre-layout netlist.

![31](https://user-images.githubusercontent.com/83575446/224399353-c2f82373-7c73-4eea-9d1c-a7d6181a08ab.png)

![image](https://user-images.githubusercontent.com/83575446/225680174-f914b402-edf5-49ea-8b1e-d87d2b568e37.png)

## Dummy Verilog Code for ADC

```
module adc(
input vin,
input vref,
input vss,
input vdd,
ouput out2);
endmodule
```
## Dummy Verilog Code for Ring Oscillator

```
module ring_osc(
input vss,
input vdd,
ouput out1);
endmodule
```
Integration of ADC & Ring_oscillator

```
module asynch_up_down(
input vss,
input vdd,
input vref,
ouput output);

wire out1;
ring_osc m1(.vss(vss),.vdd(vdd),.out1(out1));
adc m2(.vin(out1),.vref(vref),.vss(vss),.vdd(vdd),.out2(output));
endmodule
```

 # Week -6
 
 ## Created a folder `asynch_up_down-gen` inside generators folder
 
![32](https://user-images.githubusercontent.com/83575446/228045330-73d665f7-c079-4190-8d1d-7defb1911ad4.png)

 ## Dummy verilog code files

![33](https://user-images.githubusercontent.com/83575446/228045371-5a8018eb-db66-4454-811f-589a57dc741f.png)

 ## Contents inside `asynch_up_down-gen`

![34](https://user-images.githubusercontent.com/83575446/228045399-c6e3ef9b-961a-40b5-82ac-a0f5582366d9.png)

 ## Verilog Generation

![35](https://user-images.githubusercontent.com/83575446/228045454-1ba921dc-aa6c-460c-adbc-2410b8a8f646.png)

 ## Synthesis

![36](https://user-images.githubusercontent.com/83575446/228045671-d30bc0cb-cc78-4d57-8ba4-05e59895f858.png)

![37](https://user-images.githubusercontent.com/83575446/228046450-b19ada9f-b51c-4c4c-b9f1-50f2a40b8509.png)

## FloorPlanning

![38_error](https://user-images.githubusercontent.com/83575446/228046264-160d270f-69d8-4111-9877-95ed7926435a.png)

![39](https://user-images.githubusercontent.com/83575446/228046316-d61f1d39-e40a-4a98-8dce-d4a8320ef115.png)

## Global Placement

![40](https://user-images.githubusercontent.com/83575446/228046328-8a50c485-05c7-4606-8697-06f8d5a1f18a.png)

![41](https://user-images.githubusercontent.com/83575446/228046349-66ca61e5-76f2-4b6e-8cd5-94b9e88f48b0.png)

## Detail Placement

![42](https://user-images.githubusercontent.com/83575446/228046834-f3f267fb-2cd0-4eaf-a5d5-e0a118da5514.png)

![43](https://user-images.githubusercontent.com/83575446/228046850-249dc01b-df33-4326-85ea-2923139e452e.png)

## Routing

![44](https://user-images.githubusercontent.com/83575446/228046894-135ec8d5-8876-4b46-814e-b8e7bf607ee3.png)

![47](https://user-images.githubusercontent.com/83575446/228047234-aae0aec0-77c7-4bb4-986a-6358af3a20fc.png)

![45](https://user-images.githubusercontent.com/83575446/228046913-7ef8694b-73d1-4640-addb-a9f43cf4884b.png)

## DRC

![48](https://user-images.githubusercontent.com/83575446/228047251-f45f2d31-0d6d-4726-8ae6-489759f7edf3.png)

## Files Generated

![49](https://user-images.githubusercontent.com/83575446/228047262-1ea45d5e-bd2c-4bfe-be3e-2076abf0f445.png)

## Final GDSII 

![50](https://user-images.githubusercontent.com/83575446/228047272-7d88ae83-c00d-4dec-9d7a-60ba256f13e5.png)

![51](https://user-images.githubusercontent.com/83575446/228048004-cbe07a3f-27dd-46a5-8a95-625f5652847b.png)

## RTL to GDSII Done!


 # Week -7 
 # Design - Window Comparator 
 
Window Comparator utilizes two comparators in parallel to determine if a signal is between two reference voltages. If the signal is within the window, the output is high. If the signal level is outside of the window, the output is low. For this design, the reference voltages are generated from a single supply with voltage dividers.

![52](https://user-images.githubusercontent.com/83575446/229342957-5f156cbd-8019-44e1-b0d7-3f9a94278c42.png)

![53](https://user-images.githubusercontent.com/83575446/229342963-e1c7d5b7-85ec-40fa-96cf-8bb9111c1c29.png)

![54](https://user-images.githubusercontent.com/83575446/229342966-f47655e5-94c4-4197-896c-3dc17a1a4517.png)

