# uPLab4 — Pushbutton Switch Circuit

A combinational logic circuit implemented in VHDL for Intel MAX 10 FPGA that interprets pushbutton switch inputs to control two output signals: **Flip_IN** and **TRI_IN**. Built for Intel Quartus Prime using the 10M02SCU169C8G MAX 10 device.

## Overview

This project implements a **combinational logic circuit** that reads five pushbutton inputs and drives two outputs through a network of NAND and AND gates. The design is a simple but complete FPGA implementation demonstrating basic **digital logic synthesis** using VHDL.

The circuit is organized around a temporary signal (`temp`) that combines three button inputs, which then feeds into two NAND gates producing the final outputs. This structure allows a single shared condition to control both outputs simultaneously.

## Design

### Entity

```
entity uPLab4
    port(
        PH0    : in  std_logic;   -- Pushbutton H0
        PH1    : in  std_logic;   -- Pushbutton H1
        PK7    : in  std_logic;   -- Pushbutton K7
        PK6    : in  std_logic;   -- Pushbutton K6
        PH6    : in  std_logic;   -- Pushbutton H6
        TRI_IN    : out std_logic;  -- Output: Tri-state control
        Flip_IN : out std_logic   -- Output: Flip control
    );
```

### Logic Equations

The circuit implements the following Boolean logic:

```
temp    = NOT(PH6) AND NOT(PK7) AND NOT(PK6)
Flip_IN = NOT(PH0) NAND temp
TRI_IN  = NOT(PH1) NAND temp
```

Where `NOT(Pin) AND NOT(Px) AND ...` is equivalent to `NOR(Pin, Px, ...)`.

### Behavior

| PH6 | PK7 | PK6 | temp | Effect on both outputs |
|-----|-----|-----|------|------------------------|
| 0 | 0 | 0 | 1 | Both NAND gates pass through their first operand |
| 1 | X | X | 0 | temp = 0, so both NANDs output `1` (inactive) |
| X | 1 | X | 0 | temp = 0, both outputs = 1 |
| X | X | 1 | 0 | temp = 0, both outputs = 1 |

- **Flip_IN**: Asserted (low) when `PH6=0` AND `PK7=0` AND `PK6=0` AND `PH0=0` (all four buttons not pressed)
- **TRI_IN**: Asserted (low) when `PH6=0` AND `PK7=0` AND `PK6=0` AND `PH1=0`

### Version History

- **Current version** (`uPLab4.vhd`): 5-button input (PH0, PH1, PK7, PK6, PH6), uses intermediate `temp` signal with AND logic
- **Previous version** (`uPLab4.vhd.bak`): 3-button input (PH0, PH1, PK7, PK6, PH6 as inputs but simpler logic without `temp` signal), both outputs directly computed

The current version adds the intermediate `temp` signal for cleaner logic organization.

## Project Structure

```
uPlab4/
├── uPLab4.vhd            # Main VHDL design (combinational logic)
├── uPLab4.vhd.bak        # Previous simpler version (backup)
├── uPLab4.qpf            # Quartus project file
├── uPLab4.qsf            # Quartus settings (device: 10M02SCU169C8G)
├── uPLab4.qws            # Quartus workspace
├── uPLab4.qar            # Archive project package
├── uPLab4.qarlog         # Archive log
├── uPLab4.archive.rpt    # Archive report
└── .gitignore            # Excludes Quartus build artifacts
```

## Building

### Prerequisites

- **Intel Quartus Prime Lite Edition 19.1** (or later)
- **VHDL-2008** language standard
- Intel MAX 10 FPGA development board with **10M02SCU169C8G** device

### Compilation Steps

1. Open Quartus Prime → **File → Open Project**
2. Open `uPLab4.qpf` from this directory
3. Select **Project → Start Compilation** (F7)
4. Verify pin assignments in `uPLab4.qsf`

### Device Configuration

| Parameter | Value |
|-----------|-------|
| Family | MAX 10 |
| Device | 10M02SCU169C8G |
| Quartus Version | 19.1.0 Lite Edition |
| Top-level Entity | uPLab4 |
| Junction Temp Range | 0°C to 85°C |

## VHDL Conventions

- **`ieee.std_logic_1164`** — Standard logic types (std_logic, std_logic_vector)
- **`ieee.numeric_std`** — Included but not used in this combinational design
- **Single-process architecture** — All logic in concurrent signal assignments
- **Signal naming** — `temp` for intermediate computation

## References

- Intel MAX 10 Device Datasheet
- IEEE 1076 VHDL Standard
- Quartus Prime Getting Started Tutorial
