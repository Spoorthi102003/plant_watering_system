![image](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/db864408-307f-4da8-be2b-aa188fc2c58d)
# PLANT WATERING SYSTEM
## INTRODUCTION
Digital circuits can be classified as sequential and combinational crcuits
* **Combinational Circuits**:
  *Combinational circuits have no memory elements, such as flip-flops or latches.
  *The output of a combinational circuit depends only on the current inputs, with no regard for     past input values.
  *Examples include logic gates (AND, OR, XOR, etc.), multiplexers, and decoders.
* **Sequential Circuits**:
  *Sequential circuits have memory elements that store information about past inputs.
  *The output of a sequential circuit depends on both the current inputs and the internal state     (past inputs and past state).
  *Examples include flip-flops, registers, and counters.
  
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

# RTL synthesis and GLS simulation:
**Tools used in RTL netlist viewing and GLS**:
1. **iVerilog**: IVERILOG is a free and open-source Verilog simulation and synthesis tool. It's part of the Icarus Verilog project, which aims to provide a full-featured and high-performance Verilog simulation and synthesis environment.Icarus Verilog is a simulator tool to check the design with the help of test bench. The design is nothing but the Verilog hardware description language code which specifies the functionality. The testbench is the setup to apply stimulus to test the functionality of the design. This simulator looks for the changes to the input. Upon changes to the input, the output is evaluated.
2. **GTKwave** - GTKWave is a free and open-source waveform viewer. It's used primarily in digital design and verification to display simulation results generated by digital simulation tools like Icarus Verilog (which includes IVERILOG).
3. **Yosys** - Yosys is an open-source framework for Verilog RTL synthesis. It's widely used in digital design for converting high-level descriptions of a digital circuit into a gate-level representation. In other words, it helps in transforming a behavioral description (written in a language like Verilog) into a netlist, which is a detailed representation of the digital logic in terms of gates and their interconnections.

Go to the following directory and create two ".v" files
![Screenshot 2023-10-18 171830](https://github.com/Spoorthi102003/plant_watering_system/assets/143829280/082873d9-4d8d-42df-85fa-6cfdda89358d)


























