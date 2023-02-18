# VSD Mixed-signal PD Research Program - msvsdqwc

# Table of Contents  
 - [Week 1](https://github.com/Haritha266/msvsdqwc/blob/main/README.md#week-1-day-0)
 
 
 ### Install Oracle virtual box with Ubuntu 20.04
 - If you have a windows machine, install Oracle virtual box with Ubuntu 20.04 - RAM at least 4GB, hard-disk atleast 50GB 
 - Watch first 5 lectures of [VSD Open source tool installation course](https://www.udemy.com/course/vsd-a-complete-guide-to-install-open-source-eda-tools/learn/lecture/6719216#overview) to know installation procedure
 - Refer to [repo](https://github.com/kunalg123/vsdflow) for installation steps
 - Open terminal in virtual box and install git using `sudo apt-get install git`


 ### Install magic and SKY130 PDKs
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
### Netgen
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

### Xschem
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

### Ngspice
ngspice is the open-source spice simulator for electric and electronic circuits.
'sudo apt-get install ngspice'
More info can be found at [https://ngspice.sourceforge.io/index.html](https://ngspice.sourceforge.io/index.html)

Please note that to view the simulation graphs of ngspice, xterm is required and can be installed using.
```
$ sudo apt-get update
$ sudo apt-get install xterm
```
### open_pdk

Open_PDKs is distributed with files that support the Google/SkyWater sky130 open process description [https://github.com/google/skywater-pdk](https://github.com/google/skywater-pdk). Open_PDKs will set up an environment for using the SkyWater sky130 process with open-source EDA tools and tool flows such as magic, qflow, openlane, netgen, klayout, etc.<br /><br />
Install steps:

```
$  git clone git://opencircuitdesign.com/open_pdks
$  cd open_pdks
$	./configure --enable-sky130-pdk
$  make
$  sudo make install
```
### Create a lab directory folder with name inverter on desktop to perform the experiments

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
 ### Create inverter and perform pre-layout using xschem or ngspice
 
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

### Inverter Post-layout characterization using magic and SKY130 PDKs

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
This will create inverter.mag and inverter.ext files and also a .spice netlist. This .spice netlist generated post layout contains the parasitics that were absent in pre-layout netlist.

![4](https://user-images.githubusercontent.com/83575446/219821606-efc30934-0b16-4cad-87f0-7b74ec31652a.png)

Now selectively paste the control statements into postlayout netlist extracted from Magic Tool



 ```
export CC=/usr/bin/gcc
export CXX=/usr/bin/g++
git clone https://github.com/ALIGN-analoglayout/ALIGN-public
cd ALIGN-public
python3.8 -m venv general
source general/bin/activate
python3.8 -m pip install pip --upgrade
cd ALIGN-public
python3 -m pip install wheel setuptools pip --upgrade
pip install -v .
sudo apt-get -y install cmake
schematic2layout.py /home/haritha266/Desktop/ALIGN-public/ALIGN-pdk-sky130/examples/telescopic_ota -p /home/haritha266/Desktop/ALIGN-public/pdks/SKY130_PDK/


.subckt inverter vdd vin vout vss
XM1 vout vin vdd vdd sky130_fd_pr__pfet_01v8 L=0.15 W=2.6 nf=1 
XM2 vout vin vss vss sky130_fd_pr__nfet_01v8 L=0.15 W=1 nf=1 
.ends inverter

```
