# VSD Mixed-signal PD Research Program - msvsdqwc

# Table of Contents  
 - [Week 1](https://github.com/Haritha266/msvsdqwc/blob/main/README.md#week-1-day-0)
 
 
 ### Week 1 Day 1 - Install Oracle virtual box with Ubuntu 20.04
 - If you have a windows machine, install Oracle virtual box with Ubuntu 20.04 - RAM at least 4GB, hard-disk atleast 50GB 
 - Watch first 5 lectures of [VSD Open source tool installation course](https://www.udemy.com/course/vsd-a-complete-guide-to-install-open-source-eda-tools/learn/lecture/6719216#overview) to know installation procedure
 - Refer to [repo](https://github.com/kunalg123/vsdflow) for installation steps
 - Open terminal in virtual box and install git using `sudo apt-get install git`


 ### Week 1 Day 2 - Install magic and SKY130 PDKs
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

 ### Week 1 Day 3 - Install Oracle virtual box with Ubuntu 20.04
 
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


