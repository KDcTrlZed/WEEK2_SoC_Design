# VSDBABYSoC - Functional Design Overview

## Introduction

VSDBABYSoC (VSD Baby System-on-Chip) is a small yet powerful RISC-V based SoC designed for educational purposes and digital design learning. This document explains the functional architecture and design methodology of the system.

## System Architecture

### Core Components

The VSDBABYSoC integrates the following key components:

1. **RVMYTH (RISC-V CPU Core)**
   - A RISC-V based processor core
   - Executes RISC-V instruction set
   - Handles all computational tasks and control flow

2. **PLL (Phase-Locked Loop)**
   - Generates stable clock signals
   - Provides clock management for the SoC
   - Ensures timing synchronization across components

3. **DAC (Digital-to-Analog Converter)**
   - Converts digital outputs to analog signals
   - Enables interfacing with analog peripherals
   - Provides real-world signal output capability

## Functional Block Diagram

```
┌─────────────────────────────────────────┐
│          VSDBABYSoC                     │
│                                         │
│  ┌──────────┐                          │
│  │   PLL    │──────┐                   │
│  │          │      │ CLK               │
│  └──────────┘      │                   │
│                    ▼                   │
│              ┌──────────┐              │
│              │ RVMYTH   │              │
│              │ (RISC-V) │              │
│              │   Core   │              │
│              └─────┬────┘              │
│                    │                   │
│                    │ Digital Output    │
│                    ▼                   │
│              ┌──────────┐              │
│              │   DAC    │              │
│              │          │              │
│              └─────┬────┘              │
│                    │                   │
│                    ▼                   │
│              Analog Output             │
└─────────────────────────────────────────┘
```

## Design Methodology

### 1. RTL Design

The SoC is designed using Verilog HDL (Hardware Description Language). The design includes:

- **Module Hierarchy**: Top-level module `vsdbabysoc.v` integrates all sub-modules
- **Testbench**: `testbench.v` provides stimulus and verification environment
- **Include Files**: Design utilizes include directives for modular organization

### 2. Simulation Flow

Based on the terminal output, the simulation flow follows these steps:

```bash
# Compilation and Simulation
iverilog -o output/pre_synth_sim/pre_synth_sim.out \
         -DPRE_SYNTH_SIM \
         src/module/testbench.v \
         -I src/include -I src/module \
         -I output/compiled_tlv

# Execute simulation
./pre_synth_sim.out

# Waveform Analysis
gtkwave pre_synth_sim.vcd
```

**Key Simulation Parameters:**
- Simulation runs from time `[0]` to `[84999000]` (approximately 85ms)
- Uses Icarus Verilog (iverilog) for compilation
- Generates VCD (Value Change Dump) files for waveform viewing
- GTKWave is used for waveform visualization


![Image](https://github.com/user-attachments/assets/6fed6bce-2d6b-4999-97cd-149139007227)
*Figure 1: Terminal showing the complete simulation flow from compilation to waveform generation*

### 3. Signal Analysis

From the waveform viewer, the following signals are monitored:

- **CLK** (Clock): System clock signal from PLL
- **reset**: System reset signal
- **OUT** (Output): Digital output from RVMYTH core
- **VCO_IN**: Voltage Controlled Oscillator input
- **VREFH**: Reference voltage high
- **VREFL**: Reference voltage low

## Functional Operation

### Clock Generation (PLL)

1. PLL receives reference clock input
2. Generates stable, phase-locked clock signal
3. Distributes clock to RVMYTH core and other components
4. Ensures timing closure across the design

### Processor Execution (RVMYTH)

1. Fetches instructions from program memory
2. Decodes and executes RISC-V instructions
3. Performs arithmetic, logic, and control operations
4. Generates digital output based on program execution

### Digital-to-Analog Conversion (DAC)

1. Receives digital values from RVMYTH core
2. Converts digital signals to analog voltage levels
3. Uses reference voltages (VREFH, VREFL) for conversion
4. Outputs analog signal for external interfacing

## Waveform Analysis Observations
![Image](https://github.com/user-attachments/assets/c41899d9-27bd-4c16-8526-1265cad4e46a)
From the GTKWave screenshot:

- **Clock Signal**: Shows periodic toggling indicating proper PLL operation
- **Timing**: Simulation covers substantial time period (84999000 time units)
- **Signal Integrity**: Multiple signals visible including CLK, reset, and various outputs
- **Design Hierarchy**: Left panel shows module hierarchy with various signals

## Verification Strategy

1. **Pre-synthesis Simulation**: Verify RTL functionality before synthesis
2. **Waveform Analysis**: Visual inspection of signal timing and behavior
3. **VCD Dump**: Complete signal trace for debugging
4. **Testbench Coverage**: Comprehensive test scenarios in testbench.v

## File Organization

```
VSDBABYSoC/
├── src/
│   ├── module/
│   │   └── testbench.v
│   └── include/
│       └── (header files)
├── output/
│   ├── pre_synth_sim/
│   │   ├── pre_synth_sim.out
│   │   └── pre_synth_sim.vcd
│   └── compiled_tlv/
└── (other project files)
```

## Key Features

- **Educational Focus**: Designed for learning digital design and RISC-V architecture
- **Modular Design**: Clean separation between PLL, CPU, and DAC modules
- **Simulation-Ready**: Complete testbench and simulation infrastructure
- **Waveform Analysis**: GTKWave integration for detailed signal inspection
- **Mixed-Signal**: Combines digital processing with analog output capability

## Applications

- Digital design education
- RISC-V architecture learning
- Mixed-signal system understanding
- Hardware-software co-design
- RTL simulation and verification practice

## Tools Used

- **Icarus Verilog**: RTL simulation
- **GTKWave**: Waveform visualization
- **Verilog HDL**: Hardware description
- **Make**: Build automation

## Conclusion

VSDBABYSoC represents a complete, functional SoC design that integrates clock generation, RISC-V processing, and digital-to-analog conversion in a cohesive system. The design emphasizes educational value while maintaining professional design practices and verification methodology.

---

*For more information about RISC-V architecture and SoC design, visit [VLSI System Design](https://www.vlsisystemdesign.com/)*
