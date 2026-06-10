Bit-Serial TTL CPU Simulator (74HC595-style)

A browser-based hardware simulator for a minimal 5-bit bit-serial CPU architecture, designed to mimic how a real TTL breadboard computer would behave using 74-series logic (especially 74HC595 shift registers).

This project is intended as a design + prototyping tool for building the physical machine in hardware.

 Overview

This simulator models a simple custom CPU with:

5-bit word size
Bit 0 = instruction/data flag
4-bit opcode / immediate value
Bit-serial ALU execution (1 bit per clock)
74HC595-style program memory
Accumulator-based architecture
Output register (LED visualization)
FETCH → EXECUTE control cycle
 Architecture (Hardware Mapping)
Simulator Component	Hardware Equivalent
Program memory array	74HC595 shift register chain
ACC register	74HC595 / 74HC173 latch
Operand register	74HC595 / latch register
ALU	74HC86 / 74HC08 / 74HC32
Carry	74HC74 flip-flop
Clock step	Push button / 555 timer
Control FSM	74HC161 + logic gates
Output LEDs	LED bar connected to shift register
 Instruction Format (5-bit)

Each word is:

bit4 bit3 bit2 bit1 bit0
bit0 = 1 → instruction
bit0 = 0 → data
Opcodes (bits 4–1)
Instruction	Code
LOAD	0001
ADD	0010
SUB	0011
AND	0100
OR	0101
XOR	0110
OUTPUT	0111
HALT	1000
 Example Program
LOAD 3
ADD 5
XOR 2
OUTPUT
HALT
Execution result:
ACC = ((3 + 5) XOR 2)
Output register stores ACC
 Features
 Assembly-style programming

Write human-readable instructions instead of raw bits.

 Bit-serial execution model

Each instruction executes over multiple clock cycles (like real TTL hardware).

 LED visualization

Registers are displayed as:

█ · █ · █
 Step-by-step clocking
STEP button advances one clock cycle
RUN executes continuously
 Internal CPU state view

Displays:

Accumulator
Operand register
Output register
Program memory (as 74HC595 stream)
Current state (FETCH / EXEC)
 How to Use
Open sim.html in a browser
Enter assembly program
Click LOAD
Use:
STEP → single clock cycle
RUN → continuous execution
RESET → reset CPU state
 Execution Model

Each instruction works like real hardware:

FETCH phase
Read word from program memory (74HC595 chain)
Decode opcode
If needed, fetch operand word
EXEC phase (bit-serial)
Process 1 bit per clock cycle
Full 5-cycle operation per instruction
Carry stored between cycles (for ADD/SUB)
 Design Philosophy

This simulator is intentionally:

Close to real TTL behavior
Not abstract (no “instant ALU” model)
Designed for breadboard translation
Minimal chip dependency
Shift-register centric (74HC595-based architecture)
 Intended Physical Build Path

This simulator is designed to help you build:

Phase 1: Core CPU
74HC595 program register chain
5-bit accumulator
simple ALU (AND/OR/XOR/ADD)
Phase 2: Control logic
FETCH/EXEC state machine
clock distribution (555 or button)
Phase 3: Expansion
branching
memory
stack/register bank
