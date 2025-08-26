# Multiplexer-7-Segment-LED-Artix-7-FPGA-
This project demonstrates the implementation of a multiplexed 7-segment display system using an Artix-7 FPGA board (e.g., Digilent Arty A7 or EDGE Artix-7). The system leverages the Artix-7 FPGA's high-performance capabilities to control multiple 7-segment displays through a multiplexer.
Multiplexer 7-Segment Display with Artix-7 FPGA Board
Overview
This project demonstrates the implementation of a multiplexed 7-segment display system using an Artix-7 FPGA board (e.g., Digilent Arty A7 or EDGE Artix-7). The system leverages the Artix-7 FPGA's high-performance capabilities to control multiple 7-segment displays through a multiplexer, reducing the number of required I/O pins while displaying multi-digit numbers or characters efficiently.
Features

Multiplexing Technique: Drives multiple 7-segment displays using minimal FPGA I/O pins.
Artix-7 FPGA Integration: Utilizes the Xilinx Artix-7 FPGA (e.g., XC7A35T or XC7A100T) for high performance-per-watt and flexible I/O configurations.
Configurable Digits: Supports multi-digit displays (e.g., 4-digit setup).
Common Anode/Cathode Support: Compatible with both common anode and common cathode 7-segment displays.
Customizable Patterns: Easily modify display patterns for numbers, letters, or symbols via VHDL/Verilog.
Low Power Consumption: Optimized for efficiency using the Artix-7’s 28nm architecture.
Vivado Support: Fully compatible with Xilinx Vivado Design Suite (free WebPACK version).

Hardware Requirements

Artix-7 FPGA Board:
Recommended: Digilent Arty A7 (XC7A35T or XC7A100T) or EDGE Artix-7 (XC7A35T).
Features USB-JTAG programming, DDR3 SDRAM, and Pmod connectors for easy interfacing.


7-segment display modules (common anode or common cathode, e.g., 4-digit display).
Multiplexer: 74HC4051 or transistor-based multiplexing circuit.
Resistors: Current-limiting resistors (e.g., 220Ω–330Ω) for each segment.
Breadboard or PCB for circuit assembly.
Power Supply: USB-powered (5V) or external 7V-15V for Arty A7; EDGE Artix-7 supports USB or optional 5V adapter.
USB Cable: Micro B USB for programming and powering the FPGA board.

Software Requirements

Xilinx Vivado Design Suite: Free WebPACK version for programming the Artix-7 FPGA.
Optional: Digilent Adept for USB104 A7 or similar boards with specific programming needs.
VHDL/Verilog for FPGA design.
Optional: Simulation tools like Vivado Simulator or ModelSim for testing.

Circuit Setup

Connect the 7-Segment Displays:
For common anode: Connect the common pin to VCC (3.3V from Artix-7) via a current-limiting resistor.
For common cathode: Connect the common pin to GND.
Route segment pins (A–G, DP) to FPGA I/O pins or through the multiplexer.


Multiplexer Setup:
Use a 74HC4051 multiplexer or FPGA I/O pins to select active digits.
Connect multiplexer control pins to Artix-7 GPIO pins (e.g., Pmod connectors on Arty A7).


Power and Ground:
Power the Artix-7 board via USB or external supply (7V-15V for Arty A7, 5V for EDGE).
Use decoupling capacitors (e.g., 0.1µF) near the FPGA for stability.

Open the project in Xilinx Vivado Design Suite.

Synthesize and implement the design for the target Artix-7 FPGA (e.g., XC7A35T or XC7A100T).
Program the FPGA using Vivado’s Hardware Manager or store the bitstream in QSPI Flash for persistent configuration
Assemble the hardware as described in the circuit setup.


Usage

Power on the Artix-7 FPGA board.
The FPGA will multiplex the 7-segment displays to show the programmed output (e.g., numbers, countdown, or custom patterns).
Modify the VHDL/Verilog code to change display patterns or multiplexing frequency.

Example Code Snippet
module seg7(
    input clk,
    output reg[7:0]D=8'hEF,
    output reg[7:0]seg
    );
    reg clk1=0;
    integer c=0;
    reg [2:0]n=0;
    always@(posedge clk)
    begin
    c=c+1;
    if(c==100000)
    begin
    c=0;
    clk1=~clk1;
    end
    end
    always@(posedge clk1)
    begin
    n=n+1'b1;
    D={D[6:0],D[7]};
    end
    always@(n)
    begin
    case(n)
    0:seg=8'h03;
    1:seg=8'h9F;
    2:seg=8'h25;
    3:seg=8'h0D;
    4:seg=8'h99;
    5:seg=8'h49;
    6:seg=8'h41;
    7:seg=8'h1F;
    default : seg=8'hFE;
    endcase
    end      
    endmodule

Constraints File (Arty A7 Example)
# Clock signal (100 MHz on Arty A7)
set_property -dict { PACKAGE_PIN E3 IOSTANDARD LVCMOS33 } [get_ports { clk }];

# 7-segment display segments (A-G)
seg(0) = H15
seg(1) = L18
seg(2) = T11
seg(3) = P15
seg(4) = K13
seg(5) = K16
seg(6) = R10
seg(7) = T10

# Digit selection (anodes)
D(0) = J17
D(1) = J18
D(2) = T9
D(3) = J14
D(4) = P14
D(5) = T14
D(6) = K2
D(0) = U13
set_property -dict { PACKAGE_PIN G3 IOSTANDARD LVCMOS33 } [get_ports { dp }];

Inspired by Xilinx Artix-7 FPGA development projects.
Thanks to Digilent and Xilinx for comprehensive documentation and Vivado support.
Special thanks to the FPGA community for open-source resources and tutorials.
