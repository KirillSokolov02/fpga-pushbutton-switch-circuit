# FPGA 5-Button NAND/NOR Logic Circuit

VHDL implementation of a combinational logic circuit with 5 pushbutton inputs producing NAND and NOR outputs. Built with Quartus Prime 19.1.

## Overview

This project demonstrates a practical combinational logic circuit where five input pushbuttons are wired to produce outputs through NAND and NOR gate arrangements. It serves as a hands-on exercise in mapping truth tables to hardware.

## Project Structure

| File | Description |
|------|-------------|
| `uPLab4.vhd` | Top-level entity with 5 input buttons, 2 LED outputs (NAND/NOR) |

## Hardware

- **Device:** Intel MAX 10 10M02SCU169C8G
- **Board:** DE10-Lite
- **Tool:** Quartus Prime 19.1
- **Language:** VHDL-2008

## Building

1. Open `uPLab4.qpf` in Quartus Prime
2. Run Analysis & Synthesis
3. Assign pins per the board's pin catalog
4. Program the board
