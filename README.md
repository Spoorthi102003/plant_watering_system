![image](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/db864408-307f-4da8-be2b-aa188fc2c58d)
# PLANT WATERING SYSTEM
## INTRODUCTION
Digital circuits can be classified as sequential and combinational circuits
* **Combinational Circuits**:
* Combinational circuits have no memory elements, such as flip-flops or latches.
* The output of a combinational circuit depends only on the current inputs, with no regard for past input values.
* Examples include logic gates (AND, OR, XOR, etc.), multiplexers, and decoders.
* **Sequential Circuits**:
* Sequential circuits have memory elements that store information about past inputs.
* The output of a sequential circuit depends on both the current inputs and the internal state     (past inputs and past state).
* Examples include flip-flops, registers, and counters.
  
## VERILOG CODE
* Verilog is a hardware description language used for designing and simulating digital circuits. It allows engineers and designers to describe the behavior and structure of digital systems, such as integrated circuits (ICs) and field-programmable gate arrays (FPGAs). Verilog code defines how these circuits should operate, making it a crucial tool for hardware design, simulation, and verification. It is commonly used in the semiconductor industry for designing and testing complex digital systems.

## MEALY FINITE STATE MACHINE
* **Mealy state machine** is a type of finite state machine used in digital systems and control circuits. In a Mealy machine:

* **Output Depends on Inputs and State**: The machine's outputs change not only based on its current state but also in response to the inputs at that particular moment. This means that the output can change asynchronously, immediately following an input change.

* **Transitions**: State transitions in a Mealy machine are often influenced by both the current state and the current input. The decision to change from one state to another depends on this combination.

* **Timing Sensitivity**: Mealy machines can be sensitive to the timing of input changes, making them useful in situations where the output needs to react promptly to input variations.

* **Compact Design**: Mealy machines are often more compact in terms of the number of states and transitions compared to their Moore machine counterparts.

Plant watering system can be modeled as a mealy finite state machine.

# How does plant watering system work?
A plant watering system is an automated device or mechanism designed to efficiently provide water to plants, ensuring they receive the appropriate moisture levels to thrive. These systems are used in various applications, including agriculture, gardening, and indoor plant care. Here is a description of a plant watering system:

**Components of a Plant Watering System**:

**Moisture Sensor**: The heart of the system is a moisture sensor that measures the soil's moisture content. It provides feedback about whether the soil is dry or adequately moist.

**Water Source**: This can be a water reservoir or supply system, which stores water and delivers it to the plants.

**Pump**: In many automated systems, a water pump is used to transport water from the source to the plant. The pump's operation is typically controlled based on the moisture sensor's readings.

**Controller**: A controller, often a microcontroller or a programmable logic controller (PLC), processes the input from the moisture sensor and decides when and how much water to supply to the plants.

**Actuators**: Actuators, such as valves or pumps, are used to control the flow of water from the source to the plants. They are driven by the controller's instructions.

**Power Supply**: The system requires a power supply to operate, which can be battery-powered or connected to an electrical source.

**Operation of a Plant Watering System**:

**Sensing Moisture Levels**: The moisture sensor continuously monitors the soil's moisture levels. It provides data to the controller.

**Data Processing**: The controller processes the sensor data and compares it to predefined moisture level thresholds. These thresholds determine when the soil is too dry and requires watering.

**Decision-Making**: If the controller determines that the soil moisture is below the specified threshold, it activates the water supply system.

**Watering**: The water supply system, often controlled by a pump, opens to release water. The amount of water released is usually determined by the controller's settings.

**Monitoring**: After watering, the system continues to monitor the soil moisture levels. Once the sensor detects that the soil has reached an appropriate moisture level, the controller stops the water supply.

# Description of plant watering system as Mealy state machine
Verilog code for plant watering system as a Mealy state machine is given below:
```
module pes_plant_watering (
    input wire clk,         // Clock input
    input wire rst,         // Reset input
    input wire moisture_sensor, // Moisture sensor input
    output wire water_pump  // Water pump control output
);

reg [1:0] state;

// Define module-level parameters
parameter IDLE = 2'b00;
parameter WATER = 2'b01;

always @(posedge clk or posedge rst) begin
    if (rst) begin
        state <= IDLE;
    end else begin
        case(state)
            IDLE: begin
                if (moisture_sensor == 1'b0) begin
                    state <= WATER;
                end
            end
            WATER: begin
                if (moisture_sensor == 1'b1) begin
                    state <= IDLE;
                end
            end
            default: state <= IDLE;
        endcase
    end
end

assign water_pump = (state == WATER);

endmodule
```
* Output Depends on Inputs and State. In this code, the water_pump output is influenced by both the current state and the moisture_sensor input. The line assign water_pump = (state == WATER); means that the water_pump output depends on whether the current state is WATER. This output change happens immediately when the state transitions to or from WATER based on the moisture_sensor input.
* If the moisture_sensor is 1'b0(indicating moisture level low in the soil), Then the water_pump should be made high
* If the moisture_sensor is 1'b1(indicating moisture level high in the soil), Then the water_pump should be made low if it was previously high.

![Screenshot 2023-10-18 170518](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/695d8ee6-52c3-4e4f-a01c-5018bf9a82d5)

![Screenshot 2023-10-18 170540](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/dfc980f4-4d48-4715-8343-ba38201c1f89)
<details>
	
<summary> RTL synthesis and GLS simulation </summary>

**Tools used in RTL netlist viewing and GLS**:

1. **iVerilog**: IVERILOG is a free and open-source Verilog simulation and synthesis tool. It's part of the Icarus Verilog project, which aims to provide a full-featured and high-performance Verilog simulation and synthesis environment.Icarus Verilog is a simulator tool to check the design with the help of test bench. The design is nothing but the Verilog hardware description language code which specifies the functionality. The testbench is the setup to apply stimulus to test the functionality of the design. This simulator looks for the changes to the input. Upon changes to the input, the output is evaluated.
2. **GTKwave** - GTKWave is a free and open-source waveform viewer. It's used primarily in digital design and verification to display simulation results generated by digital simulation tools like Icarus Verilog (which includes IVERILOG).
3. **Yosys** - Yosys is an open-source framework for Verilog RTL synthesis. It's widely used in digital design for converting high-level descriptions of a digital circuit into a gate-level representation. In other words, it helps in transforming a behavioral description (written in a language like Verilog) into a netlist, which is a detailed representation of the digital logic in terms of gates and their interconnections.

# Installation of yosys and GTK wave

1. **Yosys**:
* Type the following to install yosys:
```
- git clone https://github.com/YosysHQ/yosys.git
- cd yosys
- sudo apt install make
- sudo apt-get update
- sudo apt-get install build-essential clang bison flex \
   libreadline-dev gawk tcl-dev libffi-dev git \
   graphviz xdot pkg-config python3 libboost-system-dev \
   libboost-python-dev libboost-filesystem-dev zlib1g-dev
- make config-gcc
- make
- sudo make install
```
2. **GTKwave**
* Type the following command to install GTKwave
```
sudo apt install gtkwave
```
3. **iVerilog**
* Type the following to install iVerilog
```
sudo apt-get install iverilog
```

To do the RTL synthesis go to the following directory and create two ".v" files
![Screenshot 2023-10-18 171830](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/082873d9-4d8d-42df-85fa-6cfdda89358d)

1. "**pes_plant_watering.v**"
Verilog code for plant watering system as a Mealy state machine is given below:
```
module pes_plant_watering (
    input wire clk,         // Clock input
    input wire rst,         // Reset input
    input wire moisture_sensor, // Moisture sensor input
    output wire water_pump  // Water pump control output
);

reg [1:0] state;

// Define module-level parameters
parameter IDLE = 2'b00;
parameter WATER = 2'b01;

always @(posedge clk or posedge rst) begin
    if (rst) begin
        state <= IDLE;
    end else begin
        case(state)
            IDLE: begin
                if (moisture_sensor == 1'b0) begin
                    state <= WATER;
                end
            end
            WATER: begin
                if (moisture_sensor == 1'b1) begin
                    state <= IDLE;
                end
            end
            default: state <= IDLE;
        endcase
    end
end

assign water_pump = (state == WATER);

endmodule
```
  ![Screenshot 2023-10-16 181320](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/e0d6255a-ef08-4c96-a115-ae4db92f456f)

2. "**pes_plant_watering_tb.v**"
The testbench code:
```
module pes_plant_watering_tb;

    reg clk;
    reg rst;
	 reg moisture_sensor;

    wire water_pump;

    pes_plant_watering uut (
        .clk(clk), 
        .rst(rst),
		  .moisture_sensor(moisture_sensor) ,
        .water_pump(water_pump)
    );

    
    initial clk = 0; 
    always #10 clk = ~clk; 

    initial begin
    
        rst = 1; 
        #50;       
        rst = 0;
		  moisture_sensor=0;
		  #10; 
		  moisture_sensor=0;
		  #10;
		  moisture_sensor=1; 
    end
    initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
    end
      
endmodule
```

  ![Screenshot 2023-10-16 181414](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/2e8e7300-7cd7-4818-9e38-09d0f32632af)


Once we have created our testbench files and the main files, we implement this in our simulation tool iVerilog.

In the `verilog_files` directory, give the following commands:
```
iverilog pes_plant_watering.v pes_plant_watering_tb.v
ls
```
We can see that an a.out executable file is created
![Screenshot 2023-10-18 173439](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/b3c0ece1-04d9-4847-9ebb-7f7f3c30bf0c)

Now we  execute the file 'a.out' by giving the following command:
```
./a.out
```
./a.out executes and we get a dump.vcd file. We run this dump.vcd file in GTK wave using the following command:
```
gtkwave dump.vcd
```

**Pre-Synthesis simulation results**
![Screenshot 2023-10-17 194308](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/7c036c4d-c8c9-4266-a0b1-d1db828c63ed)

# RTL synthesis

Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:

Converting RTL into simple logic gates.
Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
Optimizing the mapped netlist keeping the constraints set by the designer intact.
In this step we Make use of the Yosys tool to generate a Netlist, this Netlist is later run using the iverilog where the ".net" and the testbench file which give us again an executable file a.out.

Go to verilog_files directory and type `yosys`
![Screenshot 2023-10-18 174838](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/2bb558b6-8056-4ebf-8cdf-9cf35d0f9398)

Type the following commands:
```
 read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 read_verilog pes_plant_watering.v
 synth -top pes_plant_watering
```
After running the synthesis it gives us the statistics as shown below:

![Screenshot 2023-10-16 183741](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/1571e2aa-178a-4023-8e04-9700aaf0ec81)

Here are the number of components used in making plant_watering_system

To view the netlist we type the following commands:
```
 abc -liberty -lib ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
 show
```

![Screenshot 2023-10-17 194624](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/3f7fc533-0e22-457f-af19-4c0fff29514e)

Now to get the .net file, we type the following commands:
```
write_verilog pes_plant_watering_net.v
!vim pes_plant_watering_net.v
```
![Screenshot 2023-10-18 175732](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/37d99aeb-7b63-4da9-9b78-358a2b76381c)

To make the given netlist code even more simpler and small give the following commands:
```
write_verilog -noattr pes_plant_watering_net.v
!vim pes_plant_watering_net.v
```
![Screenshot 2023-10-18 180300](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/08d0c0ba-968a-47cd-99ec-edb303b5c860)

# GLS(Gate Level Simulation)

In this step we do the GLS(gate level simulation) we take the netlist file generated ".net" and the testbench file that we had written for our plant watering system at the starting and again use the iVerilog tool to generate the waveform for GLS.

To use the iVerilog, we give the following commands as shown below:
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v pes_plant_watering_net.v pes_plant_watering_tb.v
```
As we see again we have generated an executable file a.out to generate the waveform in gtkwave we execute the a.out file.

To view in gtkwave, 
```
gtkwave dump.vcd
```

![Screenshot 2023-10-17 195446](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/51848ef0-eab4-4122-9031-8b8359b6fc38)

In the above diagram, during the positive edge of the clock, if moisture_sensor is low, then water_pump goes high until the next clk cycle. It rechecks the moisture_sensor and if it is high then water_pump is made low. If not it will remain in the previous state.

We see that the output waveform is in accordance with the testbench written.

</details>

<details>
<summary> OpenLane flow </summary>

# Steps to install OpenLane, magic, Ngspice:

1. **Ngspice**:
* After downloading the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory, unpack it using:
```
 tar -zxvf ngspice-41.tar.gz
 cd ngspice-41
 mkdir release
 cd release
 ../configure  --with-x --with-readline=yes --disable-debug
 make
 sudo make install
```

2. **OpenLane**:
* Update the packages database and upgrade the packages to avoid version mismatches then install the required packages using the following commands:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
```
* In order to check the installation, you can use the following commands:

```
git --version
docker --version
python3 --version
python3 -m pip --version
make --version
python3 -m venv -h
```
* successful output should look something like this:
```
git --version
docker --version
python3 --version
python3 -m pip --version
make --version
python3 -m venv -h
git version 2.36.1
Docker version 20.10.16, build aa7e414fdc
Python 3.10.5
pip 21.0 from /usr/lib/python3.10/site-packages/pip (python 3.10)
GNU Make 4.3
Built for x86_64-pc-linux-gnu
Copyright (C) 1988-2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
usage: venv [-h] [--system-site-packages] [--symlinks | --copies] [--clear]
            [--upgrade] [--without-pip] [--prompt PROMPT] [--upgrade-deps]
            ENV_DIR [ENV_DIR ...]

Creates virtual Python environments in one or more target directories.
...
Once an environment has been created, you may wish to activate it, e.g. by
sourcing an activate script in its bin directory.
```

* Install or update docker by following the instructions given in the following link:
(https://docs.docker.com/engine/install/ubuntu/)

* To install OpenLane:
```
git clone --depth 1 https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane/
make
make test
```

A successful test will output the following line:
```
Basic test passed
```
3. **Magic**:
* To Install magic type the following commands:

```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
make
sudo make install
```

# OpenLane flow

* First, we create a folder by the name `pes_plant_watering` in designs folder. In here we create a config.json file

To do that 
```
cd OpenLane
cd designs
mkdir pes_plant_watering
cd pes_plant_watering
gedit config.json
```
* In config.json file type:
```
{
    "DESIGN_NAME": "pes_plant_watering",
    "VERILOG_FILES": "dir::src/pes_plant_watering.v",
    "CLOCK_PORT": "clk",
    "CLOCK_PERIOD": 10.0,
    "DIE_AREA": "0 0 500 500",
    "FP_SIZING": "absolute",
    "FP_PDN_VPITCH": 25,
    "FP_PDN_HPITCH": 25,
    "FP_PDN_VOFFSET": 5,
    "FP_PDN_HOFFSET": 5,
    "DESIGN_IS_CORE": true
}
``` 

![Screenshot 2023-11-04 144415](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/505ad79c-2c03-41f6-a4d9-220f564777b8)

* Create a `src` directory in the `pes_plant_watering` directory. Make a file by the name `pes_plant_watering.v` and the give the Verilog code
```
module pes_plant_watering (
    input wire clk,         // Clock input
    input wire rst,         // Reset input
    input wire moisture_sensor, // Moisture sensor input
    output wire water_pump  // Water pump control output
);

reg [1:0] state;

// Define module-level parameters
parameter IDLE = 2'b00;
parameter WATER = 2'b01;

always @(posedge clk or posedge rst) begin
    if (rst) begin
        state <= IDLE;
    end else begin
        case(state)
            IDLE: begin
                if (moisture_sensor == 1'b0) begin
                    state <= WATER;
                end
            end
            WATER: begin
                if (moisture_sensor == 1'b1) begin
                    state <= IDLE;
                end
            end
            default: state <= IDLE;
        endcase
    end
end

assign water_pump = (state == WATER);

endmodule
```

![Screenshot 2023-11-04 144732](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/36e68245-5274-402f-8760-36f414e7c396)

![Screenshot 2023-11-04 144800](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/2ee6485a-b684-4ac6-b9d4-579987055444)

## Synthesis

* In terminal type the following commands to run synthesis:
```
cd OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design pes_plant_watering
run_synthesis
```

![Screenshot 2023-11-04 145411](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/af41a4ed-1ed7-4e60-8410-63504e338a02)

![Screenshot 2023-11-04 150340](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/33837888-1e71-4999-982b-c96f3dda238f)

**Flop ratio**: 
1/3=0.333

## Floorplan

For **Floorplan** give the command: 
```
run_floorplan
```

![Screenshot 2023-11-04 150850](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/46e86406-5a18-451d-aac3-5e18f49bc07a)

To view the design:
- Go to the following directory:
```
~/Desktop/OpenLane/designs/pes_plant_watering/runs/RUN_2023.11.04_09.38.24/results/floorplan
```

- Then type the command:
```
magic -T /home/spoorthi/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read pes_plant_watering.def
```
![Screenshot 2023-11-04 151627](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/e9352b2c-6b4c-454a-8f66-6f1e05527526)

![Screenshot 2023-11-04 151413](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/261b53d9-d902-447a-9275-a775ac7d922d)

![Screenshot 2023-11-04 151458](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/04bab43f-c559-4dc2-88d3-aca8731646de)

![Screenshot 2023-11-04 151513](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/52e078e0-06ef-4e36-9be9-0dc5b485ca72)

## Placement

Give the following command:
```
run_placement
```

![Screenshot 2023-11-04 151949](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/fd5d8064-e976-470f-90ab-66e2280f7f0b)

To view the placement:
Got to the following directory
```
~/Desktop/OpenLane/designs/pes_plant_watering/runs/RUN_2023.11.04_09.38.24/results/placement
```
And type the command
```
magic -T /home/spoorthi/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read pes_plant_watering.def
```
![Screenshot 2023-11-04 153741](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/19ffbbc2-ef62-45d6-ab2c-7989827db816)

![Screenshot 2023-11-04 153517](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/5c41e7f5-1817-4252-a9a4-35529b453096)

![Screenshot 2023-11-04 153633](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/de9af6c0-ce5f-4a98-899b-355b3e1f06c2)

# Clock Tree Synthesis
To do clock tree synthesis:
```
run_cts
```
The reports are as given below:

![Screenshot 2023-11-04 160707](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/a073e624-fea7-424b-a843-eb03f16506fa)

![Screenshot 2023-11-04 160815](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/3fd66f68-c544-4639-8f16-1f82d0d53a98)

![Screenshot 2023-11-04 160901](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/7cc522ec-1788-4e87-9038-b600ab33a9ec)

Power report:
![Screenshot 2023-11-04 160936](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/bdd0c614-7963-4446-ac4c-993bdb909d4f)

skew report:
![Screenshot 2023-11-04 161003](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/c3d1ac35-ef1b-408f-b0be-b3fffd5b2969)

summary:
![Screenshot 2023-11-04 161029](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/0ab566aa-e0c0-403b-a1be-24059786bdab)



















